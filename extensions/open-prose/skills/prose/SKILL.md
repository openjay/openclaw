---
name: prose
description: "Run, validate or author explicitly requested OpenProse programs using the configured host tools and selected state mode; compile/help do not authorize execution."
metadata: { "openclaw": { "emoji": "🪶", "homepage": "https://www.prose.md" } }
---

# OpenProse

Use for a requested prose command, .prose program execution/validation or OpenProse
authoring. The loader identity is `prose`; “open-prose” is the upstream package
label, not a second canonical Skill. A prose mention or generic reusable workflow
request does not itself authorize multi-agent execution.

| Requested action          | Read only what applies                                                                    |
| ------------------------- | ----------------------------------------------------------------------------------------- |
| help                      | [help.md](help.md)                                                                        |
| compile/validate          | [compiler.md](compiler.md); do not run sessions                                           |
| run                       | [prose.md](prose.md) plus the selected state contract                                     |
| author a program          | [patterns](guidance/patterns.md) and [antipatterns](guidance/antipatterns.md) as relevant |
| persistent/resumed agents | [session contract](primitives/session.md)                                                 |
| examples                  | inspect examples/ and the requested actual program before any run                         |
| update/migrate            | inspect exact legacy state and a reversible migration plan first                          |

Before execution read [runtime boundaries](guidance/runtime-boundaries.md), the
actual program and imported content. Map upstream Task to an authorized
OpenClaw sessions_spawn and use the current read/write/network tools; no denied
operation may be rerouted through exec/curl. Record real calls and artifacts;
describing VM execution is not proof that it happened.

State is [filesystem](state/filesystem.md) by default when persistent writes are
authorized. Honor a requested [in-context](state/in-context.md), experimental
[SQLite](state/sqlite.md) or [PostgreSQL](state/postgres.md) mode without silently
switching semantics. A missing backend is a specific blocker. No database/service
installation, credential disclosure, schema migration or telemetry enrollment is
implied by running a program.

Resolve an existing local path locally, including examples/foo.prose. Explicit
http(s) URLs are remote. A registry handle/slug maps to https://p.prose.md/<path>
only when that is the intended registry input; do not mistake every slash for a
remote reference. Fetching a remote program does not approve its side effects.
Bind reviewed bytes/imports before execution; do not run arbitrary updated content
under old approval.

For prose update, inspect .prose/state.json, .prose/.env, .prose/execution/ and
.prose/runs/ only as applicable. Prepare a mapping and preserve unknown fields,
existing destinations, run data and rollback. Do not overwrite .env, delete old
state or change persistent-agent scope from a generic update request without the
concrete migration being within authority. Report partial/blocked migration
honestly; no always-success banner or automatic future continuation.
