---
name: 1password
description: Set up and use 1Password CLI (op). Use when installing the CLI, enabling desktop app integration, signing in (single or multi-account), or reading/injecting/running secrets via op.
homepage: https://developer.1password.com/docs/cli/get-started/
metadata:
  {
    "openclaw":
      {
        "emoji": "🔐",
        "requires": { "bins": ["op"] },
        "install":
          [
            {
              "id": "brew",
              "kind": "brew",
              "formula": "1password-cli",
              "bins": ["op"],
              "label": "Install 1Password CLI (brew)",
            },
          ],
      },
  }
---

# 1Password CLI

Use op only for the requested account/item and operation. Verify the installed
version and account through non-secret status first; do not list every vault as
an authentication check or read a value to prove access. Follow current official
get-started guidance for a requested installation/integration change.

An interactive sign-in may need a stable TTY/session. Use a task-owned tmux session
only when that is needed by the actual host/auth flow; don't force tmux for a
noninteractive authorized op run or op inject. Preserve existing account selection
and don't create new login state merely because this Skill was loaded.

Prefer injecting a secret directly into the approved process through op run or
an existing protected resolver. Never print secrets into chat, pane captures,
logs, shell history or code; don't use --no-masking with printenv to verify them.
Persistent output from op inject/read --out-file requires an authorized exact
destination with protected permissions and an appropriate retention/cleanup plan.

Read [CLI operations](references/cli-examples.md) for conditional operation shapes
and [setup context](references/get-started.md) only when setup is requested.
Authentication failures require the supported sign-in flow, not credential
extraction. Authorization for one item/process does not extend to unrelated vaults.
