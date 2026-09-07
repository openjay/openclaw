---
name: skill-creator
description: "Create or improve an authorized AgentSkill package with precise triggers, progressive disclosure and target-loader validation; preserve existing metadata and identities."
---

# Skill authoring

Read the current task, target loader and existing package/consumers before editing.
Keep only guidance that changes decisions: real workflow, non-obvious contracts,
inputs/outputs and authority boundaries. Generic tutorials and repeated rituals
consume context without improving a task.

Name/description should identify a precise capability and trigger. New names use
lowercase digits/letters/hyphens under 64 characters and match the directory.
Preserve existing name and supported host eligibility/invocation metadata unless
a scoped migration updates all consumers and persisted overrides. Do not remove
metadata merely because a generic validator does not understand it, or activate
a rejected historical document by silently adding description.

Use linked references for substantial conditional detail, scripts for deterministic
mechanics and assets for outputs. Do not create empty folders or copy whole manuals.
Resolve resources from the installed package. No fixed word/line target, mandatory
README deletion or blanket one-level hierarchy is an acceptance criterion.

For a new package the bundled scripts/init_skill.py accepts a name, --path,
optional --resources and --examples. Inspect the output and remove unused generated
placeholders. Do not reinitialize an existing package. Design from the user's
concrete needs without requesting examples already supplied.

Run the relevant target-loader/resource checks and meaningful positive/negative,
representative-task and permission tests. scripts/quick_validate.py checks its
declared frontmatter subset; report incompatibility instead of stripping real
contracts for a green result. scripts/package_skill.py creates a .skill archive
only when distribution is requested; inspect the actual archive and path/symlink
behavior, not a doc promise. Packaging does not install, enable or publish.

Preserve task scope and existing authorization. A Skill can guide a permitted
action, not authorize external writes, secrets, runtime mutation or unconditional
delegation. Improve observed failures with narrow fixes; do not add confirmation
loops, benchmark scores or generic audit frameworks to every request.
