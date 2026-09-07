# OpenClaw repository guidelines

Repository: https://github.com/openclaw/openclaw. Keep the root instruction layer focused on navigation, invariants and the current task. Read the matching reference only when its workflow applies.

## Scope and ownership

- Follow the current user request and applicable instruction hierarchy. Preserve unrelated staged, unstaged and untracked work. A formatting-only diff is not proof of ownership.
- Do not create/apply/drop stashes, switch branches or create/remove worktrees unless explicitly requested. An authorized isolated candidate is separate from canonical source and installed runtime.
- Commit, push, PR mutation, merge, release, deployment, external messages and destructive operations require the applicable explicit authorization. An instruction example or tool capability is not authorization; do not ask again for a step already authorized in the session.
- If a command is policy-blocked, keep its effect blocked and report the constraint. Do not substitute a lower-level command or alternate route to accomplish the denied action.
- Before security triage, read `SECURITY.md`. Before editing, inspect applicable `.github/CODEOWNERS`; security-focused paths require a listed owner's request or active review. If those rules are unavailable in this checkout, establish them before a security-sensitive edit. Keep secrets, real phone numbers, private videos and live configuration values out of output, examples and commits.
- Do not change version numbers without operator consent. Do not run npm publish/release steps without explicit authorization. Core `openclaw` uses GitHub trusted publishing; do not use `NPM_TOKEN` or the plugin OTP flow. `@openclaw/*` plugins use a separate maintainer flow, limited to already-published npm plugins; disk-only bundled plugins stay out.

## Repository map

- `src/`: TypeScript ESM core; CLI in `src/cli`, commands in `src/commands`, runtime infrastructure in `src/infra`, media in `src/media`. Tests are colocated `*.test.ts` / `*.e2e.test.ts`.
- `extensions/*`: workspace plugin packages. Keep plugin-only runtime dependencies in each package, with no `workspace:*` runtime dependency; use `openclaw` as dev/peer dependency where appropriate.
- `apps/`: native apps; `ui/`: web UI; `docs/`: public docs; `dist/`: generated output. Web installers are owned by the sibling `openclaw.ai` repository, not this checkout.
- Shared channel changes must consider built-in and extension channels, routing, pairing, allowlists, command gating and onboarding. New channels/extensions/apps/docs need matching labeler coverage; creating GitHub labels is an external action.
- Follow a more specific AGENTS in the target subtree. When adding AGENTS, also add a `CLAUDE.md` symlink to it, preserving a single body.

## Implementation and verification

- Use the checked-out `package.json` and lockfile for runtime requirements and commands. Do not treat copied version strings or a CLI on PATH as proof of the serving runtime.
- TypeScript stays strict: no `any`, `@ts-nocheck`, disabled no-explicit-any or prototype-mutation mixins. Prefer typed composition/inheritance and per-instance test stubs.
- Never hand-edit installed `node_modules`, update Carbon or patch/override/vendor dependencies without the required explicit scope. Patched dependency versions must be exact.
- Lazy module boundaries use a dedicated `*.runtime.ts` re-export boundary; do not mix static and dynamic imports of the same module in production paths. Validate changed boundaries with `pnpm build` and inspect ineffective-dynamic-import warnings.
- Use the test wrapper `pnpm test -- <path-or-filter> [args...]`; run relevant checks for the change. Keep unrun/live/fixture-only checks distinct. Before an authorized commit, run the applicable repository checks and use `scripts/committer` with exact owned paths.
- A bug-fix landing requires observed symptom evidence, code-verified root cause, an implicated-path fix and fail-before/pass-after regression or justified manual proof. Run the applicable review workflow before landing; issue text or an AI rationale alone is insufficient.
- Keep source truth, candidate checks, installed artifact, runtime state and authorization separate. Platform notes and prior smoke results are conditional historical evidence, not a current deployment declaration.

## On-demand workflows

- Implementation, tests, dependency or UI changes: [.agents/reference/development.md](.agents/reference/development.md).
- Public docs, README links or translations: [.agents/reference/documentation.md](.agents/reference/documentation.md). `docs/zh-CN/**` is generated; do not directly edit it unless explicitly requested.
- Issue triage, GitHub search, PR preparation/review or advisory handling: [.agents/reference/github-workflow.md](.agents/reference/github-workflow.md).
- Installer/Parallels/device testing or runtime diagnosis: [.agents/reference/platform-operations.md](.agents/reference/platform-operations.md). Identify the actual service before a lifecycle operation; no blanket process kill from a historical example.
- Version or release work: [.agents/reference/release.md](.agents/reference/release.md). Read the current public policy and private maintainer runbook before execution.

Load only the relevant workflow and its needed resources. Do not append all references to the always-on prompt.
