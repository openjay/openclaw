# Development and tests

Read the applicable sections for implementation, dependency, test or UI changes. Prefer commands declared in the checked-out `package.json` and lockfile over copied historical commands.

## Project Structure & Module Organization

- Source code: `src/` (CLI wiring in `src/cli`, commands in `src/commands`, web provider in `src/provider-web.ts`, infra in `src/infra`, media pipeline in `src/media`).
- Tests: colocated `*.test.ts`.
- Docs: `docs/` (images, queue, Pi config). Built output lives in `dist/`.
- Plugins/extensions: live under `extensions/*` (workspace packages). Keep plugin-only deps in the extension `package.json`; do not add them to the root `package.json` unless core uses them.
- Plugins: install runs `npm install --omit=dev` in plugin dir; runtime deps must live in `dependencies`. Avoid `workspace:*` in `dependencies` (npm install breaks); put `openclaw` in `devDependencies` or `peerDependencies` instead (runtime resolves `openclaw/plugin-sdk` via jiti alias).
- Installers served from `https://openclaw.ai/*`: live in the sibling repo `../openclaw.ai` (`public/install.sh`, `public/install-cli.sh`, `public/install.ps1`).
- Messaging channels: always consider **all** built-in + extension channels when refactoring shared logic (routing, allowlists, pairing, command gating, onboarding, docs).
  - Core channel docs: `docs/channels/`
  - Core channel code: `src/telegram`, `src/discord`, `src/slack`, `src/signal`, `src/imessage`, `src/web` (WhatsApp web), `src/channels`, `src/routing`
  - Extensions (channel plugins): `extensions/*` (e.g. `extensions/msteams`, `extensions/matrix`, `extensions/zalo`, `extensions/zalouser`, `extensions/voice-call`)
- When adding channels/extensions/apps/docs, update `.github/labeler.yml` and create matching GitHub labels (use existing channel/extension label colors).

## Local commands

- This candidate base pins `pnpm@10.23.0` and requires Node `>=22.16.0`; re-read package metadata when changing the base. Keep supported Node and Bun paths working.
- Install with the lockfile-selected package manager if dependencies are missing, then retry the failed check once. Report the first actionable failure if it still cannot run.
- `pnpm build`: build/type generation; `pnpm tsgo`: TypeScript checks; `pnpm check`: repository check suite; `pnpm test -- <path-or-filter> [args...]`: test wrapper. `pnpm test:coverage` runs coverage.
- The checked-out base has `pnpm format` as a WRITE command. Use `pnpm format:check` or the relevant `oxfmt --check` invocation for read-only verification; check scripts before running them.
- Bun is supported for TypeScript scripts and development CLI (`pnpm openclaw ...` / `pnpm dev`); Node runs built `dist/*` and production installs.
- `prek install` installs local hooks when setup is authorized. `scripts/package-mac-app.sh` packages the current architecture; it is not a publish operation.

## Coding Style & Naming Conventions

- Language: TypeScript (ESM). Prefer strict typing; avoid `any`.
- Formatting/linting via Oxlint and Oxfmt; run `pnpm check` before commits.
- Never add `@ts-nocheck` and do not disable `no-explicit-any`; fix root causes and update Oxlint/Oxfmt config only when required.
- Dynamic import guardrail: do not mix `await import("x")` and static `import ... from "x"` for the same module in production code paths. If you need lazy loading, create a dedicated `*.runtime.ts` boundary (that re-exports from `x`) and dynamically import that boundary from lazy callers only.
- Dynamic import verification: after refactors that touch lazy-loading/module boundaries, run `pnpm build` and check for `[INEFFECTIVE_DYNAMIC_IMPORT]` warnings before submitting.
- Never share class behavior via prototype mutation (`applyPrototypeMixins`, `Object.defineProperty` on `.prototype`, or exporting `Class.prototype` for merges). Use explicit inheritance/composition (`A extends B extends C`) or helper composition so TypeScript can typecheck.
- If this pattern is needed, stop and get explicit approval before shipping; default behavior is to split/refactor into an explicit class hierarchy and keep members strongly typed.
- In tests, prefer per-instance stubs over prototype mutation (`SomeClass.prototype.method = ...`) unless a test explicitly documents why prototype-level patching is required.
- Add brief code comments for tricky or non-obvious logic.
- Keep files concise; extract helpers instead of “V2” copies. Use existing patterns for CLI options and dependency injection via `createDefaultDeps`.
- Aim to keep files under ~700 LOC; guideline only (not a hard guardrail). Split/refactor when it improves clarity or testability.
- Naming: use **OpenClaw** for product/app/docs headings; use `openclaw` for CLI command, package/binary, paths, and config keys.
- Written English: use American spelling and grammar in code, comments, docs, and UI strings (e.g. "color" not "colour", "behavior" not "behaviour", "analyze" not "analyse").

## Testing Guidelines

- Framework: Vitest with V8 coverage thresholds (70% lines/branches/functions/statements).
- Naming: match source names with `*.test.ts`; e2e in `*.e2e.test.ts`.
- Run `pnpm test` (or `pnpm test:coverage`) before pushing when you touch logic.
- For targeted/local debugging, keep using the wrapper: `pnpm test -- <path-or-filter> [vitest args...]` (for example `pnpm test -- src/commands/onboard-search.test.ts -t "shows registered plugin providers"`); do not default to raw `pnpm vitest run ...` because it bypasses wrapper config/profile/pool routing.
- Do not set test workers above 16; tried already.
- If local Vitest runs cause memory pressure (common on non-Mac-Studio hosts), use `OPENCLAW_TEST_PROFILE=low OPENCLAW_TEST_SERIAL_GATEWAY=1 pnpm test` for land/gate runs.
- Live tests (real keys): `CLAWDBOT_LIVE_TEST=1 pnpm test:live` (OpenClaw-only) or `LIVE=1 pnpm test:live` (includes provider live tests). Docker: `pnpm test:docker:live-models`, `pnpm test:docker:live-gateway`. Onboarding Docker E2E: `pnpm test:docker:onboard`.
- Full kit + what’s covered: `docs/help/testing.md`.
- Changelog: user-facing changes only; no internal/meta notes (version alignment, appcast reminders, release process).
- Changelog placement: in the active version block, append new entries to the end of the target section (`### Changes` or `### Fixes`); do not insert new entries at the top of a section.
- Changelog attribution: use at most one contributor mention per line; prefer `Thanks @author` and do not also add `by @author` on the same entry.
- Pure test additions/fixes generally do **not** need a changelog entry unless they alter user-facing behavior or the user asks for one.
- Mobile: before using a simulator, check for connected real devices (iOS + Android) and prefer them when available.

## Implementation seams

- CLI progress uses `src/cli/progress.ts`; status tables use `src/terminal/table.ts` with ANSI-safe wrapping; terminal colors use `src/terminal/palette.ts`. `status --all` is read-only/pasteable; `status --deep` performs probes.
- A new connection provider must update relevant macOS/web/mobile UI, onboarding/docs, status and configuration forms; keep discovery and settings consistent.
- SwiftUI: prefer Observation (`@Observable`, `@Bindable`); introduce `ObservableObject` only for compatibility, and migrate touched related usages when appropriate.
- Tool schemas targeting google-antigravity must avoid Type.Union/anyOf/oneOf/allOf, raw `format` property names and nullable optional fields. Use stringEnum/optionalStringEnum and Type.Optional; top-level object plus properties.
- A2UI `.bundle.hash` is generated. Regenerate only through `pnpm canvas:a2ui:bundle` or `scripts/bundle-a2ui.sh` when needed; keep the generated hash change separately scoped for review/commit.
- Never change the Carbon dependency. Patched dependencies require exact versions. Dependency patches, overrides and vendored changes require explicit approval before shipping.
- Do not hand-edit `node_modules` in any install. Do not create prototype-mutation mixins or bypass typing with `@ts-nocheck`/disabled no-explicit-any. Use typed composition/inheritance and existing dependency-injection patterns.
