# GitHub workflow

Read for issue triage, PR preparation/review or explicitly authorized GitHub actions. Read-only analysis does not authorize labels, comments, closing, merging or publishing. Existing exact-scope authorization need not be requested again.

## Communication and review

- Use repo-relative file references for OpenClaw discussions unless the host requires absolute clickable file links. Include the full issue/PR URL when reporting that work.
- Write multiline GitHub bodies to a file and pass `--body-file` (or a single-quoted heredoc with `-F -`); never interpolate backticks or shell metacharacters into `-b`. Use real newlines, not literal `\n`.
- Keep issue/PR numbers unquoted for auto-linking. In landing comments, make source and landed SHAs clickable with full commit URLs.
- Address and resolve bot conversations on an authorized PR once fixed. Leave conversations that require reviewer/maintainer judgment unresolved.
- Search targeted keywords before duplicating work. Use `gh search issues/prs --repo openclaw/openclaw --match title,body`; add comments when relevant. Pagination must cover the requested scope; a limited sample is not an exhaustive search.

## Auto-close labels (issues and PRs)

- If an issue/PR matches one of the reasons below, apply the label and let `.github/workflows/auto-response.yml` handle comment/close/lock.
- Do not manually close + manually comment for these reasons.
- Why: keeps wording consistent, preserves automation behavior (`state_reason`, locking), and keeps triage/reporting searchable by label.
- `r:*` labels can be used on both issues and PRs.

- `r: skill`: close with guidance to publish skills on Clawhub.
- `r: support`: close with redirect to Discord support + stuck FAQ.
- `r: no-ci-pr`: close test-fix-only PRs for failing `main` CI and post the standard explanation.
- `r: too-many-prs`: close when author exceeds active PR limit.
- `r: testflight`: close requests asking for TestFlight access/builds. OpenClaw does not provide TestFlight distribution yet, so use the standard response (“Not available, build from source.”) instead of ad-hoc replies.
- `r: third-party-extension`: close with guidance to ship as third-party plugin.
- `r: moltbook`: close + lock as off-topic (not affiliated).
- `r: spam`: close + lock as spam (`lock_reason: spam`).
- `invalid`: close invalid items (issues are closed as `not_planned`; PRs are closed).
- `dirty`: close PRs with too many unrelated/unexpected changes (PR-only label).

## PR truthfulness and bug-fix validation

- Never merge a bug-fix PR based only on issue text, PR text, or AI rationale.
- Before `/landpr`, run `/reviewpr` and require explicit evidence for bug-fix claims.
- Minimum merge gate for bug-fix PRs:
  1. symptom evidence (repro/log/failing test),
  2. verified root cause in code with file/line,
  3. fix touches the implicated code path,
  4. regression test (fail before/pass after) when feasible; if not feasible, include manual verification proof and why no test was added.
- If claim is unsubstantiated or likely hallucinated/BS: do not merge. Request evidence/changes, or close with `invalid` when appropriate.
- If linked issue appears wrong/outdated, correct triage first; do not merge speculative fixes.

## Commits and landing

- Follow the maintainer-specified workflow. `.agents/maintainers.md` routes to the maintained workflow repository; do not assume the retired `.agents/skills/PR_WORKFLOW.md` exists locally. If `/reviewpr` or `/landpr` is required, resolve the current workflow before the corresponding action.
- For an explicitly authorized commit, use `scripts/committer "<msg>" <file...>` to scope staging; use concise action-oriented messages. Keep unrelated changes out.
- Templates: `.github/pull_request_template.md` and `.github/ISSUE_TEMPLATE/`.
- `sync` means inspect the requested scope and integrate it only within the actions already authorized. It does not authorize committing all dirty files or pushing. Never include other authors' work merely because it is formatting-only. If an authorized rebase conflicts, preserve work and resolve only within scope; stop at unsafe overlap.
- A blocked branch deletion stays blocked. Report the constraint; never substitute a lower-level ref deletion command to achieve the same denied effect.
- For an authorized bulk PR close/reopen affecting more than five PRs, require authorization that explicitly covers the exact count and target scope/query before executing.

## Security advisories

Read `SECURITY.md` and obey security-focused CODEOWNERS before advisory analysis or changes. The advisory workflow's commands are not publication authority.

- Read an advisory with `gh api /repos/openclaw/openclaw/security-advisories/<GHSA>` when the task permits it.
- Before an authorized publication, private-fork PRs must be closed and `severity`, `description` and `vulnerabilities` populated. Write Markdown and JSON to files rather than shell-interpolating bodies.
- The API cannot set `severity` and `cvss_vector_string` in one PATCH; use separate authorized calls. Publication is a PATCH with `state: published`, not a `/publish` endpoint.
- For HTTP 422 inspect missing fields and open private-fork PRs. After publication re-fetch and verify `state`, `published_at`, content and newline integrity.
