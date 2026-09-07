---
name: peekaboo
description: Capture and automate macOS UI with the Peekaboo CLI.
homepage: https://peekaboo.boo
metadata:
  {
    "openclaw":
      {
        "emoji": "👀",
        "os": ["darwin"],
        "requires": { "bins": ["peekaboo"] },
        "install":
          [
            {
              "id": "brew",
              "kind": "brew",
              "formula": "steipete/tap/peekaboo",
              "bins": ["peekaboo"],
              "label": "Install Peekaboo (brew)",
            },
          ],
      },
  }
---

# Peekaboo

Use the Peekaboo CLI for requested macOS UI inspection/automation when this is the
appropriate available surface. Screen Recording/Accessibility and tool access
must already permit the operation; a Skill does not grant system permissions.

Discover the installed version and relevant subcommand help. Inspect the target
app/window using list/see, then act on the observed snapshot/element and verify
the result. Refresh after navigation or layout changes; never invent target IDs
or reuse a stale snapshot. Prefer explicit app/window/snapshot targeting.

Read [targeting guide](references/targeting.md) for the retained command families.
Use screenshots only for the task's necessary surface. Avoid broad clipboard or
screen capture that exposes unrelated private content; never place credentials
in CLI arguments or logs.

Opening, clicking or pressing Return can submit a form or send a message. Match
the authorized target/content/action before crossing that boundary; do not ask
again when the exact action is already authorized. Stop on uncertain target,
unexpected sensitive action or permission denial rather than escalating access.
Configuration changes, app termination, cache cleanup and service installs are
separate operations, not routine UI prerequisites.
