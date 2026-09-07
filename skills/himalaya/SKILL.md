---
name: himalaya
description: "CLI to manage emails via IMAP/SMTP. Use `himalaya` to list, read, write, reply, forward, search, and organize emails from the terminal. Supports multiple accounts and message composition with MML (MIME Meta Language)."
homepage: https://github.com/pimalaya/himalaya
metadata:
  {
    "openclaw":
      {
        "emoji": "📧",
        "requires": { "bins": ["himalaya"] },
        "install":
          [
            {
              "id": "brew",
              "kind": "brew",
              "formula": "himalaya",
              "bins": ["himalaya"],
              "label": "Install Himalaya (brew)",
            },
          ],
      },
  }
---

# Himalaya email

Use an existing configured account for the requested folder/message operation.
Check installed help/version and select --account explicitly when ambiguous.
IDs are folder-relative: re-list after changing folders and verify sender,
recipients and thread before editing/replying. Limit searches to the task scope.

Common read paths: folder list, envelope list with folder/page/page-size/search,
message read/export, attachment download. Inspect whether the installed read
operation marks a message seen; don't call it mutation-free without evidence.
Keep downloads in the authorized location and don't expose private mail in logs.

For composing use [MML reference](references/message-composition.md); preserve
To/Cc/Bcc, subject, reply identity and exact approved attachments. Interactive
compose/reply/forward and template send can transmit when an editor exits or a
command runs. Drafting is not sending, and reading is not permission to move,
delete or change flags. Reuse explicit action authority already given; resolve
only missing material details before mutation.

Read [configuration](references/configuration.md) only for authorized setup.
Use protected password commands/keyring, not inline secret configs or chat.
Do not turn an auth failure into account reconfiguration. Avoid raw trace logs
that expose headers, messages or credentials; preserve a sanitized error category.
After an uncertain send inspect the existing message before retrying. Return
actual results; no “sent” claim from a draft or launched editor alone.
