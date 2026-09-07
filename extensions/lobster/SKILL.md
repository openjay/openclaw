# Lobster — source guide

This source document has no discovery frontmatter and remains unadmitted as a
Skill. The optional Lobster tool/plugin is a separate configured capability.
Use it only when the requested workflow and current tool policy permit execution.

Inspect a pipeline's actual steps, dependencies, outputs and side effects before
run. Repeatable orchestration does not imply deterministic external data or LLM
outputs. An email triage request is not permission to send replies or register a
morning scheduler. A tool allowlist entry does not supply task authority.

The documented tool envelope uses protocolVersion, ok, status, output and
requiresApproval. A needs_approval result includes a prompt/items and resumeToken.
Keep that token bound to the exact reviewed items; resume with approve=true only
when authorization covers them. A pause/resume token is not itself consent.
Do not bypass a denied bridge tool through another pipeline or direct command.

Verify actual output/receipts after resume and preserve unknown/failure states.
Scheduling, enabling the plugin, installing lobster, changing tool allowlists or
opening /tools/invoke is a separate authorized operation. Read README.md for
integration context, not as permission to enable arbitrary tools.
