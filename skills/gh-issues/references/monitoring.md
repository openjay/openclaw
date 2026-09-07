# Monitoring and worker handoff

An authorized watch stores exact issue/PR/run/comment identities, scope and stop
conditions. Use the host's actual scheduler for requested future execution; a
CLI flag or narrative is not a registered job. Stay quiet when state is unchanged
unless periodic status was requested. Stop on cancellation, scope/budget expiry
or an action requiring new authority. Existing approval is not a license for
unbounded new issues, repositories or review requests.

Use the actual state location/owner; the historical /data/.clawdbot claims/cursor
paths are not universal host paths. Preserve claim lineage. Do not infer worker
termination from age or a missing PR. Check actual run state before retry/reclaim.
Cron mode can launch one bounded child only where its scheduler contract includes
that authority and durable tracking. Report “spawned” separately from “fixed”,
“pushed” and “PR opened”.

Worker evidence: changed paths/HEAD, observed symptom, source root cause, relevant
test commands/results, remaining criteria, and external receipts if those actions
were authorized. Per-comment replies and final-channel notifications are external
messages. --notify-channel is not permission to send every worker transcript or
intermediate update. Do not include tokens, raw private logs or hidden reasoning.
