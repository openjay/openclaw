---
description: Review a PR and its evidence without modifying or merging it
---

Review PR $1 (number or URL). If omitted, use an unambiguous PR from this conversation; ask only if the target cannot be resolved.

This command is review-only. Do not edit code, change checkout/branch, assign or label the PR, send a comment, push or merge. Read metadata, full relevant diff and surrounding code using `gh pr view`, `gh pr diff` and read-only Git operations; requesting review does not authorize `gh pr checkout` in a shared worktree.

Read `.agents/reference/github-workflow.md` for the applicable evidence and security-owner rules. For each material claim, verify the symptom, code location/root cause, why the changed path addresses it and regression coverage (fail before/pass after when feasible, otherwise justified manual proof). Missing evidence is NEEDS WORK; reserve INVALID CLAIM for evidence that actually falsifies the claim.

Inspect correctness, concurrency, error handling, contracts/compatibility, security/privacy and the tests relevant to the changed path. Keep findings actionable with exact file/line and a concrete consequence; distinguish blockers from optional improvements. Do not propose unrelated refactoring to fill a review template.

Return READY FOR LANDING REVIEW, NEEDS WORK, INVALID CLAIM or NEEDS DISCUSSION with concise rationale, claim/evidence mapping, actionable findings, tests actually run and unverified paths. Readiness is a review verdict, not merge authority. Include the full PR URL. A draft comment may be included when requested; posting it requires authorization.
