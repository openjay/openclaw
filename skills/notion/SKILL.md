---
name: notion
description: Notion API for creating and managing pages, databases, and blocks.
homepage: https://developers.notion.com
metadata:
  {
    "openclaw":
      { "emoji": "📝", "requires": { "env": ["NOTION_API_KEY"] }, "primaryEnv": "NOTION_API_KEY" },
  }
---

# Notion API

Use an existing authorized Notion connector/client for requested page, database,
data-source or block work. Identify the exact page/container and read its schema
and existing content before a write. A local draft is not a remote page mutation.
Do not create an integration, share a page or acquire/store credentials as an
implicit prerequisite; use the protected configured NOTION_API_KEY resolver.

The source package recorded API version 2025-09-03. It is a pinned historical
reference, not a claim of latest support. Consult that version's primary API
documentation for the actual operation; preserve distinct database_id,
data_source_id, page_id and block_id meanings. Do not guess parent shapes or
rename all databases to data sources based on a summary.

Typical workflow: search the scoped target → retrieve metadata/schema and blocks
with pagination → prepare a precise property/block change → execute only the
authorized mutation → read back affected content. Preserve property types,
relationships, existing structure and unrelated user edits. Add/update/archive/
delete/sharing operations have separate scope; replace-all is not a default edit.

Send structured JSON through the supported client rather than interpolating user
text into shell/JSON. Bound pagination and retries; inspect Retry-After on rate
limits. An uncertain write requires reconciliation before retry. Never print API
keys, auth headers or broad private page contents for diagnostics. Report actual
IDs/results and any unverified capability rather than fabricated success.
