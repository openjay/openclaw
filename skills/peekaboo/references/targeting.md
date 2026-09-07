# Peekaboo targeting guide

Use installed `peekaboo <command> --help` for the actual version. This is a compact
index of the existing package's CLI families, not a guarantee that every flag is
present.

- Inspection: permissions, list apps/windows/screens, see --annotate, image.
- Targeting: --app, --pid, --window-title/--window-id; --snapshot from a current
  see result; --on/--id for an observed element, coordinates only when grounded.
- Interaction: click, type, press, hotkey, scroll, drag, swipe. Check the relevant
  syntax and resulting UI before chaining dependent actions.
- Specialized work: menu/menubar/dock, dialog, window/space, app, capture live,
  clipboard and run scripts are conditional capabilities with distinct effects.
- JSON output is available in the recorded package; verify --json/-j per command.

Do not copy an example login password, automatically use polter for “fresh builds”
or assume --return is harmless. A local automation script can contain arbitrary
UI mutations; inspect it before invoking run.
