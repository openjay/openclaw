---
description: Prepare and land an explicitly authorized PR after review and exact-head checks
---

Target: PR $1 (number or URL), or the unambiguous PR already named in the conversation.

1. Establish which actions the user authorized for that exact PR. A request to prepare/audit landing is not a merge request. When landing is explicitly authorized, continue through the approved steps without asking again; author-branch rewriting, unrelated metadata changes and cleanup are not implicit prerequisites.
2. Read `.agents/reference/github-workflow.md`, the root/nested AGENTS and the current maintainer workflow routed by `.agents/maintainers.md`. Run the applicable review workflow first. Do not invent an inaccessible runbook.
3. Bind the PR's current head/base SHAs and actual diff. Verify bug claims with symptom/root-cause evidence and relevant regression proof. Check draft state, required reviews, required CI and mergeability for that head. A change of head invalidates older head-specific checks.
4. Preserve all existing staged/unstaged/untracked work. Do not switch branches, create stashes or force-push the author's branch to manufacture a clean checkout. Use an already-authorized isolated candidate if fixes are needed; otherwise report the exact preparation action needed. Make only requested fixes and run the applicable pinned checks before an authorized scoped commit.
5. Re-read the remote PR state immediately before the authorized merge. Prefer squash unless the user's/maintainer's history requirement calls for rebase. If evidence or required checks fail, stop at HOLD; never bypass branch protection or close the PR as a substitute for merging.
6. Verify GitHub state is MERGED and record the head/merge commit URLs. If merge status is uncertain, query before retrying. Report the result and exact validation. Post a landing comment, sync another checkout or delete temporary branches only if those actions are included in the authorization; use file-backed multiline comment bodies.
