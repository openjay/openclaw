---
name: oracle
description: Best practices for using the oracle CLI (prompt + file bundling, engines, sessions, and file attachment patterns).
homepage: https://askoracle.dev
metadata:
  {
    "openclaw":
      {
        "emoji": "🧿",
        "requires": { "bins": ["oracle"] },
        "install":
          [
            {
              "id": "node",
              "kind": "node",
              "package": "@steipete/oracle",
              "bins": ["oracle"],
              "label": "Install oracle (node)",
            },
          ],
      },
  }
---

# Oracle request preparation

Use the requested installed oracle CLI to bundle an explicit question and the
smallest relevant file set for another model. Honor the user's engine/model and
data-sharing scope. Do not choose a historical GPT default, install via npx or
launch a remote browser host just because the binary is absent.

Inspect installed help and use its payload/file-report preview before a request.
Select exact relevant files; verify exclusions, symlinks, size limits and actual
expanded payload. No secrets, private transcripts or unrelated files may be
uploaded by a broad src/\*\* or config glob. Copy-to-clipboard is also a side effect
and can expose the whole bundle.

Provide the task, source evidence, relevant environment/build/test facts, prior
attempts, constraints and requested output. Keep hidden reasoning out; use a
concise decision summary and observable evidence. API/browser engines can have
different cost, upload and privacy behavior; do not silently switch destinations.

Record session/slug and actual submission outcome. If the run detaches or times
out, inspect/reattach through the installed session commands before retrying;
don't pay for or submit a duplicate prompt on uncertainty. Do not force a new
run or expose remote auth tokens in CLI arguments as a fallback.

Treat returned analysis as advisory. Verify concrete claims against code/tests
before applying changes or publishing. Report prepared/submitted/completed states
separately; the Skill is not proof of current model availability or serving state.
