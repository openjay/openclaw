# Platform operations

Read only for an authorized installer smoke test, device build or runtime diagnosis on the named platform. This material was extracted from the source instruction overlay reviewed on 2026-09-06. Version- and snapshot-specific observations below are historical expectations that must be requalified against the requested artifact; they do not prove the current host or service state. Snapshot restore, installs, SSH, service restart and channel probes require their applicable task authority.

## Runtime identity and lifecycle

Identify the actual process, executable, service manager and version before using a lifecycle command. Do not assume the source note's menubar-only deployment describes this host; LaunchAgent/npm/fork deployments can differ. For a verified Mac-app-managed gateway, use the app or the maintained `scripts/restart-mac.sh` workflow when authorized, not an ad-hoc tmux daemon. Remove only task-owned temporary tunnels on completion. Use `scripts/clawlog.sh` for the applicable unified logs and inspect its privilege requirements.

For exe.dev operations, establish the actual target via the authorized SSH/web-terminal path. Verify local gateway mode, credential format and service manager. Use its scoped lifecycle/runbook; never use a blanket process-name kill command copied from an old example. Verify the listener and RPC result separately. A request phrased “update fly” still needs the exact app/machine and permitted update/restart scope; historical host IDs are not safe defaults.

## Parallels smoke details

- Vocabulary: "makeup" means "mac app".
- Parallels macOS retests: use the snapshot most closely named like `macOS 26.3.1 fresh` when the user asks for a clean/fresh macOS rerun; avoid older Tahoe snapshots unless explicitly requested.
- Parallels beta smoke: use `--target-package-spec openclaw@<beta-version>` for the beta artifact, and pin the stable side with both `--install-version <stable-version>` and `--latest-version <stable-version>` for upgrade runs. npm dist-tags can move mid-run.
- Parallels beta smoke, Windows nuance: old stable `2026.3.12` still prints the Unicode Windows onboarding banner, so mojibake during the stable precheck log is expected there. Judge the beta package by the post-upgrade lane.
- Parallels macOS smoke playbook:
  - `prlctl exec` is fine for deterministic repo commands, but it can misrepresent interactive shell behavior (`PATH`, `HOME`, `curl | bash`, shebang resolution). For installer parity or shell-sensitive repros, prefer the guest Terminal or `prlctl enter`.
  - Fresh Tahoe snapshot current reality: `brew` exists, `node` may not be on `PATH` in noninteractive guest exec. Use absolute `/opt/homebrew/bin/node` for repo/CLI runs when needed.
  - Preferred automation entrypoint: `pnpm test:parallels:macos`. It restores the snapshot most closely matching `macOS 26.3.1 fresh`, serves the current `main` tarball from the host, then runs fresh-install and latest-release-to-main smoke lanes.
  - Gateway verification in smoke runs should use `openclaw gateway status --deep --require-rpc`, not plain `--deep`, so probe failures go non-zero.
  - Latest-release pre-upgrade diagnostics still need compatibility fallback: stable `2026.3.12` does not know `--require-rpc`, so precheck status dumps should fall back to plain `gateway status --deep` until the guest is upgraded.
  - Harness output: pass `--json` for machine-readable summary; per-phase logs land under `/tmp/openclaw-parallels-smoke.*`.
  - All-OS parallel runs should share the host `dist` build via `/tmp/openclaw-parallels-build.lock` instead of rebuilding three times.
  - Current expected outcome on latest stable pre-upgrade: `precheck=latest-ref-fail` is normal on `2026.3.12`; treat it as a baseline signal, not a regression, unless the post-upgrade `main` lane also fails.
  - Fresh host-served tgz install: restore fresh snapshot, install tgz as guest root with `HOME=/var/root`, then run onboarding as the desktop user via `prlctl exec --current-user`.
  - For `openclaw onboard --non-interactive --secret-input-mode ref --install-daemon`, expect env-backed auth-profile refs (for example `OPENAI_API_KEY`) to be copied into the service env at install time; this path was fixed and should stay green.
  - Don’t run local + gateway agent turns in parallel on the same fresh workspace/session; they can collide on the session lock. Run sequentially.
  - Root-installed tarball smoke on Tahoe can still log plugin blocks for world-writable `extensions/*` under `/opt/homebrew/lib/node_modules/openclaw`; treat that as separate from onboarding/gateway health unless the task is plugin loading.
- Parallels Windows smoke playbook:
  - Preferred automation entrypoint: `pnpm test:parallels:windows`. It restores the snapshot most closely matching `pre-openclaw-native-e2e-2026-03-12`, serves the current `main` tarball from the host, then runs fresh-install and latest-release-to-main smoke lanes.
  - Gateway verification in smoke runs should use `openclaw gateway status --deep --require-rpc`, not plain `--deep`, so probe failures go non-zero.
  - Latest-release pre-upgrade diagnostics still need compatibility fallback: stable `2026.3.12` does not know `--require-rpc`, so precheck status dumps should fall back to plain `gateway status --deep` until the guest is upgraded.
  - Always use `prlctl exec --current-user` for Windows guest runs; plain `prlctl exec` lands in `NT AUTHORITY\SYSTEM` and does not match the real desktop-user install path.
  - Prefer explicit `npm.cmd` / `openclaw.cmd`. Bare `npm` / `openclaw` in PowerShell can hit the `.ps1` shim and fail under restrictive execution policy.
  - Use PowerShell only as the transport (`powershell.exe -NoProfile -ExecutionPolicy Bypass`) and call the `.cmd` shims explicitly from inside it.
  - Harness output: pass `--json` for machine-readable summary; per-phase logs land under `/tmp/openclaw-parallels-windows.*`.
  - Current expected outcome on latest stable pre-upgrade: `precheck=latest-ref-fail` is normal on `2026.3.12`; treat it as a baseline signal, not a regression, unless the post-upgrade `main` lane also fails.
  - Keep Windows onboarding/status text ASCII-clean in logs. Fancy punctuation in banners shows up as mojibake through the current guest PowerShell capture path.
- Parallels Linux smoke playbook:
  - Preferred automation entrypoint: `pnpm test:parallels:linux`. It restores the snapshot most closely matching `fresh` on `Ubuntu 24.04.3 ARM64`, serves the current `main` tarball from the host, then runs fresh-install and latest-release-to-main smoke lanes.
  - Use plain `prlctl exec` on this snapshot. `--current-user` is not the right transport there.
  - Fresh snapshot reality: `curl` is missing and `apt-get update` can fail on clock skew. Bootstrap with `apt-get -o Acquire::Check-Date=false update` and install `curl ca-certificates` before testing installer paths.
  - Fresh `main` tgz smoke on Linux still needs the latest-release installer first, because this snapshot has no Node/npm before bootstrap. The harness does stable bootstrap first, then overlays current `main`.
  - This snapshot does not have a usable `systemd --user` session. Treat managed daemon install as unsupported here; use `--skip-health`, then verify with direct `openclaw gateway run --bind loopback --port 18789 --force`.
  - Env-backed auth refs are still fine, but any direct shell launch (`openclaw gateway run`, `openclaw agent --local`, Linux `gateway status --deep` against that direct run) must inherit the referenced env vars in the same shell.
  - `prlctl exec` reaps detached Linux child processes on this snapshot, so a background `openclaw gateway run` launched from automation is not a trustworthy smoke path. The harness verifies installer + `agent --local`; do direct gateway checks only from an interactive guest shell when needed.

## Device and session details

- Check connected physical iOS/Android devices before choosing a simulator. “Restart apps” means rebuild/install/relaunch when that is the requested test, not merely kill/launch.
- Do not rebuild the macOS app over SSH; run the build directly on the Mac.
- iOS signing-team lookup is an authorized local signing/setup action; use Xcode identities/settings rather than hardcoding a team.
- For a requested agent session log, use the current Runtime agent ID to select `~/.openclaw/agents/<agentId>/sessions/*.jsonl`, the newest unless an ID is specified. Do not substitute `sessions.json` or search unrelated private sessions. Remote reads require the target and access to be in scope.
- For rebrand/migration diagnosis, inspect `docs/gateway/doctor.md` and the command's effects before using `openclaw doctor`.
- Keep external WhatsApp/Telegram delivery final-only; streaming/tool events may remain in internal UIs. Message sending still requires authorization.
- Voice wake command remains `openclaw-mac agent --message "${text}" --thinking low`; VoiceWakeForwarder escapes the message already. Keep launchd PATH adequate for the verified install without adding duplicate quoting.
