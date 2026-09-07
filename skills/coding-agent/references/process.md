# Process handoff and monitoring

Use the available host exec schema: command, explicit workdir, bounded timeout,
optional background mode and PTY only for a CLI mode that requires a terminal.
Inspect actual CLI help before choosing flags. Never escalate a blocked operation
through a different harness or a wrapper.

Record process/session ID, target checkout/HEAD, child scope and output locations.
The process tool may support list/poll/log/write/submit/send-keys/kill; use only the
current exposed actions. Sending input can approve a mutation. Inspect the exact
prompt and existing authorization before responding; no automatic y/yes replay.

For parallel jobs use disjoint file ownership and the configured concurrency
limit. Preserve parent-child lineage and useful failures. A timeout after submit
requires reconciliation, not immediate respawn. End with source changes, observed
tests, incomplete criteria and any separately gated publication/runtime step.
