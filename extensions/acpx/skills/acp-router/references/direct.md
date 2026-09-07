# Direct acpx sessions

The source extension pins its acpx dependency in extensions/acpx/package.json.
Prefer that plugin-local binary over an unrelated global executable when the
approved installation exists. Inspect its help/version before composing commands.
Do not invoke npx adapters that install missing packages without installation
scope; configured aliases/overrides are part of the user's environment.

Use --cwd for the intended repository and a stable session name such as
oc-<harness>-<actual-conversation-id>. Inspect sessions show before sessions new;
a failed lookup due to auth/backend failure is not proof of absence. Persistent
prompting uses the actual session ID/name; exec mode is for a requested one-shot.
quiet output may help relaying, but retain failure/receipt status separately.

The recorded CLI families are <harness> sessions show/new/close, persistent
<harness> -s <session>, <harness> exec and <harness> cancel. Exact flags depend on
the pinned version. NO_SESSION can justify creating the requested missing session;
queue busy calls for waiting/status, not duplicate creation. Installing a binary,
restoring an override or restarting Gateway is a separate repair operation.
