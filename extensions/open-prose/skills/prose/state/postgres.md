---
role: postgres-state-management
status: experimental
summary: |
  PostgreSQL-based state management for OpenProse programs. This approach persists
  execution state to a PostgreSQL database, enabling true concurrent writes,
  network access, team collaboration, and high-throughput workloads.
requires: psql CLI tool in PATH, running PostgreSQL server
see-also:
  - ../prose.md: VM execution semantics
  - filesystem.md: File-based state (default, simpler)
  - sqlite.md: SQLite state (queryable, single-file)
  - in-context.md: In-context state (for simple programs)
  - ../primitives/session.md: Session context and compaction guidelines
---

# PostgreSQL state — experimental, unverified backend

Select only for an explicit PostgreSQL-mode request. The former documentation
passed credentialed connection strings to child prompts/logs and treated those
credentials as non-sensitive. That path is not permitted. This candidate changes
the instructions; it does not implement a new secure state transport or validate
a running database.

Before execution require a configured, authorized backend with protected credential
resolution that keeps passwords/tokens out of model context, command arguments,
narration and SQL text. Only a non-secret service/reference handle may be passed
to children. Missing protected transport is a blocker; do not ask to view .env,
echo OPENPROSE_POSTGRES_URL or fall back to a production DATABASE_URL.

Read [historical schema sketch](postgres-schema-reference.md) only for backend
design review. It has not been database-tested and includes unresolved expression
key/conflict-target syntax. Do not run it as a migration or claim PostgreSQL mode
is ready. No trust-auth Docker service, port exposure, package install, schema
creation/ALTER or cleanup follows from reading this reference.

The intended ownership remains: VM owns run/execution state; children own their
scoped binding and permitted agent/segment outputs. Keep run/project/user scopes
distinct, use an existing verified transactional writer with parameterized data,
and preserve state after uncertainty for reconciliation. A shared schema does
not provide per-agent isolation by itself.

If prerequisites are missing, report the exact backend/authority/schema gap and
offer an appropriate existing state mode for a separate user decision; do not
silently change persisted semantics. No performance, durability or concurrency
claim is certified by this document. See [runtime boundaries](../guidance/runtime-boundaries.md).
