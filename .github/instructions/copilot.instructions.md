---
applyTo: "**/*.ts,**/*.tsx"
---

# OpenClaw TypeScript patterns

Follow the root and applicable nested AGENTS for authority, module boundaries and verification. Read `.agents/reference/development.md` when the task needs implementation or test detail.

- Reuse the existing implementation when it matches the required semantics. Search before adding a formatter/helper; use `src/infra/format-time.ts` for supported time formats rather than duplicating them.
- Use TypeScript ESM with `.js` import specifiers and `import type` for type-only imports. Prefer direct imports; a dedicated `*.runtime.ts` re-export boundary is valid for lazy loading, and public SDK boundaries may require re-exports.
- Terminal tables use `src/terminal/table.ts`; progress uses `src/cli/progress.ts`; colors use the applicable shared theme/palette. CLI wiring lives in `src/cli`, commands in `src/commands`, dependencies use `createDefaultDeps`.
- Use the pinned package manager and scripts from `package.json`. Run focused tests via `pnpm test -- <path-or-filter>` and relevant type/lint/build checks. Before an authorized commit, use `scripts/committer` with only the owned paths; pairing with a human does not change staging ownership.
