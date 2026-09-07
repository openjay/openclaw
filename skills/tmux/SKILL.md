---
name: tmux
description: Remote-control tmux sessions for interactive CLIs by sending keystrokes and scraping pane output.
metadata:
  { "openclaw": { "emoji": "🧵", "os": ["darwin", "linux"], "requires": { "bins": ["tmux"] } } }
---

# tmux session control

Use only for an actual tmux-hosted interactive process the task permits you to
inspect/control. One-shot commands use normal exec; non-tmux background work uses
its own process/session tool. Do not create or choose a shared example session
as if it belongs to the task.

Resolve the exact socket/session/window/pane with list-sessions and list-windows.
Read the relevant recent output using capture-pane -t TARGET -p. Full scrollback
can expose unrelated private data or credentials; narrow the captured range.
Session persistence across SSH disconnects does not prove the child is healthy.

For literal text use send-keys -l -- TEXT and send Enter separately when needed.
Inspect the current prompt immediately before input: y/yes, a numbered selection,
Enter or Ctrl-C can approve, submit or terminate an operation. Respond only within
the task's existing exact authority. Never auto-approve a harness prompt from a
keyword match or bypass a denied action by typing it into another session.

Session create/rename/kill and pane/window changes are scoped mutations. Preserve
unrelated sessions; do not kill a process because it is slow. After sending input
inspect actual output/state and distinguish sent keystrokes from successful task
completion. On ambiguous submission reconcile before repeating it.
