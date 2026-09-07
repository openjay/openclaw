# Feishu block operations

Use the single feishu_doc tool with action, not obsolete per-action tool names.
Read list_blocks/get_block to identify actual block IDs and structure. Examples
are field contracts; use the installed tool schema for its exact version.

| Operation                | Fields                                                                          |
| ------------------------ | ------------------------------------------------------------------------------- |
| update_block             | doc_token, block_id, content                                                    |
| insert                   | doc_token, after_block_id, content                                              |
| delete_block             | doc_token, exact block_id                                                       |
| create_table             | doc_token, row_size, column_size; optional column_width/parent_block_id         |
| write_table_cells        | doc_token, table_block_id, values matrix                                        |
| create_table_with_values | doc_token, row_size, column_size, values; optional column_width/parent_block_id |
| upload_image             | doc_token and source url or file_path; optional parent_block_id/index           |
| upload_file              | doc_token, exactly one url/file_path; optional filename/parent_block_id         |

Markdown tables are not a substitute for a native table action. Do not describe
all programmatic table creation as unavailable when native actions exist. Verify
table dimensions/values and the installed supported action before execution.

Text/heading/list/code/callout blocks use their supported text editor. Containers
retain child relationships; edit the relevant child instead of replacing the
whole document. Images and files require their supported upload/edit route and
authorized content. Index is 0-based where exposed by the tool, not a guessed
page number. Avoid delete-and-rewrite as a generic insertion workaround.

Read back affected blocks. The full upstream block-type catalog changes; use
the actual returned block_type and version-specific primary API reference for an
unfamiliar type instead of copying stale numeric assumptions.
