---
name: coding-agent
description: "Delegate explicitly requested substantial coding work to an available coding harness within the authorized repository, tools and budget; ACP thread requests use acp-router."
metadata:
  {
    "openclaw": { "emoji": "🧩", "requires": { "anyBins": ["claude", "codex", "opencode", "pi"] } },
  }
---

# Coding harness delegation

Use for substantial coding work when delegation is requested or authorized.
Honor the chosen harness; simple edits or source reading can be done directly.
For a requested ACP harness thread, use `acp-router` and its sessions_spawn path.
Do not substitute terminal scraping for a working ACP integration.

Identify the actual installed CLI, relevant help, project instructions and safe
working directory before launch. Noninteractive and interactive modes differ
between CLIs/versions; a PTY is conditional, not a universal Codex requirement.
Do not invent a current default model from an old example. Keep existing sandbox
and approval settings; no default --yolo, bypassPermissions, elevation or automatic
“yes” response. Tool capability does not authorize changing approval policy.

Give each child a bounded objective, owned paths, constraints, acceptance criteria,
budget and required artifacts. Do not pass credentials/private context unrelated
to the task. Working directory selects context; it is not a filesystem security
boundary. Keep execution out of the live OpenClaw state/runtime and preserve dirty
work. New worktrees/branches require the applicable task scope.

Use the host's supported exec/process tools and record session ID. Monitor existing
work without duplicate launch; do not kill merely for being slow. On failure or
uncertain completion, inspect state before retrying a mutation. Do not silently
take over a task the user required the named harness to perform.

Verify actual artifacts/tests before reporting completion. A child summary is a
claim, not proof. Commit, push, PR/comment publication, cleanup and OpenClaw system
event notifications occur only when explicitly within scope. Background launch
does not authorize a future scheduler or imply work continues after it exits.
Read [process contract](references/process.md) for handoff/monitoring fields.
