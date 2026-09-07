---
name: acp-router
description: "Route explicit coding-harness work to the configured ACP runtime or an authorized direct acpx session; thread creation uses sessions_spawn runtime acp."
user-invocable: false
---

# ACP harness router

Use for an explicit request to run/continue work in a named ACP harness. Default
to the configured OpenClaw ACP runtime. For a requested harness thread use
sessions_spawn with runtime="acp", thread=true, mode="session" (or requested
one-shot mode), the requested task and explicit agentId when no default is known.
Do not create the thread through message(thread-create), subagent runtime or PTY
scraping. A general mention of Codex/Claude is not a request to start a harness.

Map the requested name to the configured agent alias: pi, claude, codex, opencode,
gemini or kimi where available. Preserve user tool/model choice and surface an
actual policy rejection. Do not route around it through direct exec.

Use direct acpx only when explicitly requested or an authorized equivalent fits
the task and the runtime's lifecycle features are unnecessary. Read
[direct session contract](references/direct.md). Reuse the correct existing
conversation/session ID and working directory; do not create a new session just
because a previous result is late or queue-busy.

If the backend/binary/adapter is missing, inspect pinned package dependencies and
the safe version/status evidence. A thread-start request does not authorize npm
installation, global install, deleting agent overrides, changing configuration or
Gateway restart. Prepare the exact repair and rollback if needed; execute it only
within existing explicit repair authority, then retry the same request once when
appropriate. Preserve user overrides and report missing prerequisites honestly.

Relay the actual final harness result and useful sanitized failure information.
Do not claim a session or task succeeded from process launch alone. Cancel/close
only the intended session within scope; no unattended future work from a prompt.
