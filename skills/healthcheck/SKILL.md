---
name: healthcheck
description: "Audit or perform explicitly authorized OpenClaw host hardening, exposure and version checks while preserving access and separating diagnosis from system changes."
---

# OpenClaw host posture

Establish the host/container, OS, role, actual Gateway and the user's access path
from available evidence. Ask only for material missing context; a request for an
audit authorizes its relevant read-only checks and does not require a second
permission ritual. Avoid unrelated credential reads or broad private inventory.

Use installed help and current primary OS/OpenClaw documentation for exact check
semantics. Distinguish local inspection from --deep probes or remote contact.
Collect relevant bind/listener, firewall, access, version and update evidence for
the requested scope; optional backup/encryption context must not trigger backup
creation, provider setup or an unrelated audit by itself.

For remediation, present the concrete target state, changes, access-preservation
steps, rollback and verification. Match each action to existing authorization;
do not reconfirm every already-approved step. Firewall/ports, SSH/RDP, package/
service/user changes, update policy, schedules and sensitive credential access
need explicit action scope. New lockout risk or an unexpected result stops the
affected action. Keep an authorized recovery path before changing remote access.

OpenClaw security audit --fix affects its documented configuration/permissions;
it does not administer the host firewall, SSH or OS updates. Verify the exact
version's behavior before running it. Never bypass a denied command by changing
tool policy or using another execution route.

Recheck only the affected posture and access path, then report commands/results,
changed files and unresolved risks. Existing green health or a version match is
not proof of exposure or authentication safety. No blanket model upgrade, choice
menu or mandatory scheduler offer belongs in every audit.

If periodic checks are requested, inspect existing jobs and configure only the
approved cadence/output/notification scope. Otherwise end with the current audit.
Memory writes occur only with explicit opt-in in an appropriate private workspace;
append redacted current-date records and preserve existing entries/ownership.
