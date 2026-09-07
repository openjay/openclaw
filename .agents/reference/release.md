# Release work

Read only when preparing or executing an explicitly requested release. Version edits, tagging, publish and production activation each remain bound to their actual authorization. Existing consent need not be repeated, but one authorized operation does not imply the others.

Use `docs/reference/RELEASING.md` for public policy and the private [maintainer release runbook](https://github.com/openclaw/maintainers/blob/main/release/README.md) for actual auth/signing/release mechanics. If the runbook is inaccessible, that release path remains unverified; do not reconstruct private credential steps from older history.

## Release Channels (Naming)

- stable: tagged releases only (e.g. `vYYYY.M.D`), npm dist-tag `latest`.
- beta: prerelease tags `vYYYY.M.D-beta.N`, npm dist-tag `beta` (may ship without macOS app).
- beta naming: prefer `-beta.N`; do not mint new `-1/-2` betas. Legacy `vYYYY.M.D-<patch>` and `vYYYY.M.D.beta.N` remain recognized.
- dev: moving head on `main` (no tag; git checkout main).

## Versions and artifacts

- Version locations include package.json, Android versionName/versionCode, iOS app/test CFBundle versions, macOS Info.plist, the pinned npm version in `docs/install/updating.md`, and relevant Peekaboo project/Info.plist values. Inventory actual paths in the revision before an authorized version change.
- “Bump everywhere” excludes `appcast.xml`; update that only when cutting the macOS Sparkle release.
- Beta Git tags `vYYYY.M.D-beta.N` must publish the matching `YYYY.M.D-beta.N` npm version, not a plain version with only `--tag beta`, which consumes that plain version name.

## Release Auth

- Core `openclaw` publish uses GitHub trusted publishing; do not use `NPM_TOKEN` or the plugin OTP flow for core releases.
- Separate `@openclaw/*` plugin publishes use a different maintainer-only auth flow.
- Plugin scope: only publish already-on-npm `@openclaw/*` plugins. Bundled disk-tree-only plugins stay out.
- Maintainers: private 1Password item names, tmux rules, plugin publish helpers, and local mac signing/notary setup live in the private [maintainer release docs](https://github.com/openclaw/maintainers/blob/main/release/README.md).

## Changelog Release Notes

- When cutting a mac release with beta GitHub prerelease:
  - Tag `vYYYY.M.D-beta.N` from the release commit (example: `v2026.2.15-beta.1`).
  - Create prerelease with title `openclaw YYYY.M.D-beta.N`.
  - Use release notes from `CHANGELOG.md` version section (`Changes` + `Fixes`, no title duplicate).
  - Attach at least `OpenClaw-YYYY.M.D.zip` and `OpenClaw-YYYY.M.D.dSYM.zip`; include `.dmg` if available.

- Keep top version entries in `CHANGELOG.md` sorted by impact:
  - `### Changes` first.
  - `### Fixes` deduped and ranked with user-facing fixes first.
- Before tagging/publishing, run:
  - `node --import tsx scripts/release-check.ts`
  - `pnpm release:check`
  - `pnpm test:install:smoke` or `OPENCLAW_INSTALL_SMOKE_SKIP_NONROOT=1 pnpm test:install:smoke` for non-root smoke path.

Check command availability in the pinned revision. Tests, npm version comparisons and artifact checks prove only their exact scope; they do not grant publish authority or establish that deployed runtime changed.
