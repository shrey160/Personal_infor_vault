# Obsidian MCP — operational notes (2026-09-10)

> _Source: this workspace file. Mirror to the Obsidian vault under `Chat Workspace/Knowledge/dshtools/`._

Field notes from an end-to-end test of the `mcp__obsidian__*` bridge against the
live vault (Local REST API `127.0.0.1:27123`, bound at `dsh web` boot).

## What works (verified this session)
- **Read/nav:** `vault_list`, `vault_read` (full + targeted heading/block/frontmatter),
  `vault_get_document_map` (heading tree, block IDs, `version` token), `active_file_get_path`,
  `open_file`.
- **Search:** `search_simple` (relevance + match context), `search_query` (JsonLogic —
  glob/var/== usable), `tag_list` (reflects tags live).
- **Write/edit:** `vault_write`, `vault_append`, `vault_copy`, `vault_move`,
  `vault_patch` (heading append, frontmatter list-append + createTargetIfMissing,
  frontmatter scalar replace, block append), `vault_delete` (moves to trash).
- **Commands:** `command_list`, `command_execute` (e.g. `editor:save-file`).
- `[[wikilink]]` → appears in `unresolvedLinks` until target exists.

## Quirk 1 — Heading targeting by array path is flaky
- Only the **document root / top-level heading** resolves: `["Note Title"]` works.
- **Nested** paths like `["Section One"]` or `["A","B"]` consistently return
  `Target not found: heading [...]` — reproduced on both a scratch file and a real
  session note. Server-side resolver quirk, not misuse.
- **Workaround:** target the root heading and express the new heading level
  *relative* in `content` (a leading `#` = direct child of the root).

## Quirk 2 — Block-scope list append splices without a newline
- Appending `- item three` to a 2-item list via a block `append` produced
  `- item two- item three` on one line. Functional but not prettified.
- **Workaround:** for list continuation, include your own leading `\n` in the payload,
  or target the heading (root) instead of the block.

## Patterns that work cleanly
- Frontmatter array append with `createTargetIfMissing: true` merges cleanly
  (`tags: [...]`) and shows up in `tag_list` immediately.
- `vault_delete` sends to trash per Obsidian deleted-files config (not permanent);
  pass `permanent: true` only when explicitly needed.
- Test artifacts were created under `Chat Workspace/_tests/` and then deleted —
  vault left clean.