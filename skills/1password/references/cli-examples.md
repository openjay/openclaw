# op CLI operations

Use installed op help for exact flags. Keep secret references out of output where
their names reveal sensitive context. These examples use symbolic identifiers,
not live accounts or secrets.

- Status: op whoami; use --account/OP_ACCOUNT for the intended account.
- Sign-in: op signin through the approved app/session flow when setup is authorized.
- Process injection: op run with the approved executable and reference-based
  environment; the process must consume values without printing them.
- Template injection: op inject -i TEMPLATE -o APPROVED_OUTPUT only when storing
  the rendered secret file is required and authorized.
- Reading an item: target the exact approved op:// reference through the protected
  consumer. Do not issue a plaintext read into tool/chat output as a smoke test.

Never use op run --no-masking -- printenv or capture a tmux pane containing a
secret. Listing vaults/accounts or exporting private keys is not a default
verification step for an unrelated operation.
