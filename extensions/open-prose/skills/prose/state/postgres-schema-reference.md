# Historical PostgreSQL schema sketch — unvalidated

Preserved from the tracked experimental source for an explicit backend design
review. This is not an executable migration or a verified schema. In particular,
the expression-based PRIMARY KEY/UNIQUE and matching conflict targets require
PostgreSQL syntax/design verification. Do not initialize a database from this
sketch or report backend readiness. A schema repair needs its own isolated test,
migration/compatibility review and deployment authority.

## Core Schema

The VM initializes these tables using the `openprose` schema. This is a **minimum viable schema**—extend freely.

```sql
-- Create dedicated schema for OpenProse state
CREATE SCHEMA IF NOT EXISTS openprose;

-- Run metadata
CREATE TABLE IF NOT EXISTS openprose.run (
    id TEXT PRIMARY KEY,
    program_path TEXT,
    program_source TEXT,
    started_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    status TEXT NOT NULL DEFAULT 'running'
        CHECK (status IN ('running', 'completed', 'failed', 'interrupted')),
    state_mode TEXT NOT NULL DEFAULT 'postgres',
    metadata JSONB DEFAULT '{}'::jsonb
);

-- Execution position and history
CREATE TABLE IF NOT EXISTS openprose.execution (
    id SERIAL PRIMARY KEY,
    run_id TEXT NOT NULL REFERENCES openprose.run(id) ON DELETE CASCADE,
    statement_index INTEGER NOT NULL,
    statement_text TEXT,
    status TEXT NOT NULL DEFAULT 'pending'
        CHECK (status IN ('pending', 'executing', 'completed', 'failed', 'skipped')),
    started_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    error_message TEXT,
    parent_id INTEGER REFERENCES openprose.execution(id) ON DELETE CASCADE,
    metadata JSONB DEFAULT '{}'::jsonb
);

-- All named values (input, output, let, const)
CREATE TABLE IF NOT EXISTS openprose.bindings (
    name TEXT NOT NULL,
    run_id TEXT NOT NULL REFERENCES openprose.run(id) ON DELETE CASCADE,
    execution_id INTEGER,  -- NULL for root scope, non-null for block invocations
    kind TEXT NOT NULL CHECK (kind IN ('input', 'output', 'let', 'const')),
    value TEXT,
    source_statement TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    attachment_path TEXT,
    metadata JSONB DEFAULT '{}'::jsonb,
    PRIMARY KEY (name, run_id, COALESCE(execution_id, -1))  -- Composite key with scope
);

-- Persistent agent memory
CREATE TABLE IF NOT EXISTS openprose.agents (
    name TEXT NOT NULL,
    run_id TEXT,  -- NULL for project-scoped and user-scoped agents
    scope TEXT NOT NULL CHECK (scope IN ('execution', 'project', 'user', 'custom')),
    memory TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    metadata JSONB DEFAULT '{}'::jsonb,
    PRIMARY KEY (name, COALESCE(run_id, '__project__'))
);

-- Agent invocation history
CREATE TABLE IF NOT EXISTS openprose.agent_segments (
    id SERIAL PRIMARY KEY,
    agent_name TEXT NOT NULL,
    run_id TEXT,  -- NULL for project-scoped agents
    segment_number INTEGER NOT NULL,
    timestamp TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    prompt TEXT,
    summary TEXT,
    metadata JSONB DEFAULT '{}'::jsonb,
    UNIQUE (agent_name, COALESCE(run_id, '__project__'), segment_number)
);

-- Import registry
CREATE TABLE IF NOT EXISTS openprose.imports (
    alias TEXT NOT NULL,
    run_id TEXT NOT NULL REFERENCES openprose.run(id) ON DELETE CASCADE,
    source_url TEXT NOT NULL,
    fetched_at TIMESTAMPTZ,
    inputs_schema JSONB,
    outputs_schema JSONB,
    content_hash TEXT,
    metadata JSONB DEFAULT '{}'::jsonb,
    PRIMARY KEY (alias, run_id)
);

-- Indexes for common queries
CREATE INDEX IF NOT EXISTS idx_execution_run_id ON openprose.execution(run_id);
CREATE INDEX IF NOT EXISTS idx_execution_status ON openprose.execution(status);
CREATE INDEX IF NOT EXISTS idx_execution_parent_id ON openprose.execution(parent_id) WHERE parent_id IS NOT NULL;
CREATE INDEX IF NOT EXISTS idx_execution_metadata_gin ON openprose.execution USING GIN (metadata jsonb_path_ops);
CREATE INDEX IF NOT EXISTS idx_bindings_run_id ON openprose.bindings(run_id);
CREATE INDEX IF NOT EXISTS idx_bindings_execution_id ON openprose.bindings(execution_id) WHERE execution_id IS NOT NULL;
CREATE INDEX IF NOT EXISTS idx_agents_run_id ON openprose.agents(run_id) WHERE run_id IS NOT NULL;
CREATE INDEX IF NOT EXISTS idx_agents_project_scoped ON openprose.agents(name) WHERE run_id IS NULL;
CREATE INDEX IF NOT EXISTS idx_agent_segments_lookup ON openprose.agent_segments(agent_name, run_id);
```

### Schema Conventions

- **Timestamps**: Use `TIMESTAMPTZ` with `NOW()` (timezone-aware)
- **JSON fields**: Use `JSONB` for structured data in `metadata` columns (queryable, indexable)
- **Large values**: If a binding value exceeds ~100KB, write to `attachments/{name}.md` and store path
- **Extension tables**: Prefix with `x_` (e.g., `x_metrics`, `x_audit_log`)
- **Anonymous bindings**: Sessions without explicit capture use auto-generated names: `anon_001`, `anon_002`, etc.
- **Import bindings**: Prefix with import alias for scoping: `research.findings`, `research.sources`
- **Scoped bindings**: Use `execution_id` column—NULL for root scope, non-null for block invocations

### Scope Resolution Query

For recursive blocks, bindings are scoped to their execution frame. Resolve variables by walking up the call stack:

```sql
-- Find binding 'result' starting from execution_id 43 in run '20260116-143052-a7b3c9'
WITH RECURSIVE scope_chain AS (
  -- Start with current execution
  SELECT id, parent_id FROM openprose.execution WHERE id = 43
  UNION ALL
  -- Walk up to parent
  SELECT e.id, e.parent_id
  FROM openprose.execution e
  JOIN scope_chain s ON e.id = s.parent_id
)
SELECT b.* FROM openprose.bindings b
WHERE b.name = 'result'
  AND b.run_id = '20260116-143052-a7b3c9'
  AND (b.execution_id IN (SELECT id FROM scope_chain) OR b.execution_id IS NULL)
ORDER BY
  CASE WHEN b.execution_id IS NULL THEN 1 ELSE 0 END,  -- Prefer scoped over root
  b.execution_id DESC NULLS LAST  -- Prefer deeper (more local) scope
LIMIT 1;
```

**Simpler version if you know the scope chain:**

```sql
-- Direct lookup: check current scope (43), then parent (42), then root (NULL)
SELECT * FROM openprose.bindings
WHERE name = 'result'
  AND run_id = '20260116-143052-a7b3c9'
  AND (execution_id = 43 OR execution_id = 42 OR execution_id IS NULL)
ORDER BY execution_id DESC NULLS LAST
LIMIT 1;
```

---
