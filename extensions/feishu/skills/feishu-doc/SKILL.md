---
name: feishu-doc
description: "Read or edit an explicitly referenced Feishu/Lark document through feishu_doc; use for Feishu docx URLs, not generic local Word files."
---

# Feishu documents

Use feishu_doc for the requested Feishu/Lark document. Extract the doc_token from
the actual docx URL and start with action=read. If hint/block_types indicates
tables/images or other structure, use list_blocks/get_block before editing;
plain text is not the full document topology.

Prefer update_block for a scoped text edit, append for requested additions and
delete_block only for the exact authorized block. action=write replaces the whole
document: use only for a requested full replacement with the existing structure
and user's unrelated content accounted for. Markdown image URLs can upload media;
do not substitute a destructive write to repair a single image/table.

In this checkout, create uses title, optional folder_token and optional
`grant_to_requester`; requester identity comes from trusted runtime context and
its default grants edit permission. Do not supply an invented owner_open_id or
claim this grants full_access. Creating a document and changing other collaborators
are separate scopes; verify the actual deployed schema before using another version.

For tables/images/attachments read [block operations](references/block-types.md).
The installed tool action schema owns supported fields and limits; confirm it
before using a parameter from another version. No enabling tool/config scopes or
changing document sharing merely because a read request fails.

Verify affected content/blocks and visibility after the authorized change.
Preserve actual doc/block IDs and safe receipts; an API acknowledgement does not
prove visual layout. Don't leak token/config contents or claim unsupported edits.
