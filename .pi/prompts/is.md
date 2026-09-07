---
description: Analyze GitHub issues without implementing changes
---

Analyze GitHub issue(s): $ARGUMENTS.

Read the issue, relevant comments and linked evidence. Treat the reported symptom and proposed root cause as hypotheses: verify them against the current implicated code path, tests and available reproduction evidence. Follow only links and files needed to establish the claim.

For a bug, identify the verified cause, its code location, a minimal fix and the regression case that would distinguish it. For a feature request, identify the user need, existing behavior, affected contracts and a bounded implementation approach. Separate unsupported claims and missing evidence from confirmed findings.

Report the issue URL, finding, evidence and proposed next step. Analyze only; do not edit, assign, label, comment, close, commit or push unless separately requested.
