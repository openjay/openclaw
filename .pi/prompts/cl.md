---
description: Audit OpenClaw changelog coverage for a release or commit range
---

Audit changelog coverage for $ARGUMENTS. This is an audit unless the user also requests edits.

1. Resolve the requested release/range. If omitted, inspect reachable release tags and `CHANGELOG.md`; state the chosen base and distinguish stable from beta. Version-sort order alone does not establish the relevant release.
2. Read the commits and changed paths in that range. Match user-visible changes to entries in the actual root `CHANGELOG.md` release block. Skip internal housekeeping and pure tests unless they affect behavior or the user requested an entry.
3. Report missing, inaccurate or duplicate entries with commit/PR evidence. Follow OpenClaw's `Changes` / `Fixes` convention and append within the target section. Keep at most one contributor attribution per entry.
4. If edits were requested, make the scoped changelog corrections and verify links/attribution. Do not create unrelated package changelogs or a speculative feature section. No commit, tag, publish or release follows from this audit.
