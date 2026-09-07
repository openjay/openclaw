---
name: node-connect
description: Diagnose OpenClaw node connection and pairing failures for Android, iOS, and macOS companion apps. Use when QR/setup code/manual connect fails, local Wi-Fi works but VPS/tailnet does not, or errors mention pairing required, unauthorized, bootstrap token invalid or expired, gateway.bind, gateway.remote.url, Tailscale, or plugins.entries.device-pair.config.publicUrl.
---

# Node connection diagnosis

Determine the intended route—same host/emulator/tunnel, LAN, tailnet or public
reverse proxy—from existing context before changing anything. Read the exact app
error and known Gateway/node identity. Ask only for information needed to resolve
an ambiguous route; don't redirect a LAN task to Tailscale by default.

Use installed OpenClaw help and scoped config/status checks for advertised URL,
bind/mode, auth mode and actual pairing state. Prefer non-secret fields. QR/setup
payloads may contain bootstrap credentials and generating them may create new
pairing state; don't dump the full JSON into chat as a routine read-only check.
Share only the setup material the authorized user needs via the intended surface.

Distinguish route reachability, authentication, pending pairing and connected node
state. “Pairing required” suggests a pending stage; it does not identify which
device to approve. Verify exact request/device identity and existing authorization
before approval. Never approve --latest without resolving the intended request.

A diagnosis does not authorize binding to LAN/public interfaces, Tailscale Serve/
Funnel, disabling auth, replacing credentials, changing remote URLs or restarting
Gateway. Prepare one evidence-supported repair with access/rollback implications;
apply only steps already explicitly authorized. Missing/expired setup code is not
permission to regenerate all pairing/auth state.

Report observed endpoints without secrets, exact failure stage and next bounded
action. Do not claim reachability from config alone or infer correct auth from a
successful local command. Inspect actual results after an authorized change.
