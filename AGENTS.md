# AGENTS.md — operating instructions for this chat project

> **Current date: 2026-09-10** — always re-verify with the present date before
> beginning any research or web search; do not trust stale remembered dates.

This is a persistent chat workspace for research, learning, and exploration. Its
memory lives in markdown files. You (the agent) are responsible for keeping that
memory accurate, current, and small.

## File Responsibilities

- **`personality.md`** — how you behave: tone, depth, format rules, working modes.
  Read it at session start. It changes only when the user explicitly asks or when a
  correction is repeated twice.
- **`user.md`** — durable facts about the user: background, interests, active
  projects, preferences. Read it at session start. Update in place when facts change.
- **`memory/short_term.md`** — the current session's staging area: what was discussed,
  decisions made, things to follow up. Cleared down during consolidation. Budget:
  ~100 lines.
- **`memory/long_term.md`** — distilled, durable knowledge across all sessions:
  stable preferences, concluded findings, recurring constraints. One line per entry,
  each linking to its detail file. Budget: ~150 lines. Rewrite to compress; never
  stack duplicates.
- **`memory/long_term_topics/<topic>.md`** — the full story behind a `long_term.md`
  entry: what was concluded, why, key evidence, what changed since. Editable whenever
  the topic resurfaces — this is the look-back layer.
- **`memory/topics/<thread>.md`** — one file per *active* research/learning thread
  (status, key findings with dates, open questions, next steps, links to artifacts).
  Create on demand; mark `status: parked` when the user moves on; on conclusion,
  promote to `long_term.md` + `long_term_topics/`.
- **`memory/sessions.md`** — append-only session log: date, one-line summary,
  files touched. Never edit old entries. Never decide from this file.
- **`knowledge/<domain>/...`** — content the conversations produce, organised by
  domain (ai/, robotics/, design/, …) with free nesting. File anything meant to be
  re-read: explainers, roadmaps, comparisons, design docs, code. Keep each domain's
  `_index.md` current — one line per file.

## Obsidian Vault Sync (only when MCP is active)

The workspace has an MCP bridge to the user's Obsidian vault (Local REST API on
`127.0.0.1:27123`, configured in the DSH profile patch at
`~/.dsh/profiles/web/cordis.patch.yml`). The bridge binds at `dsh web` boot, so
`mcp__obsidian__*` tools are present in some sessions and absent in others.

1. **Gate at session start.** After the memory reads below, check this session's
   toolset. If `mcp__obsidian__*` tools exist → vault duty ON: jot
   `obsidian-mcp: on` into `memory/short_term.md`. If not → vault duty OFF:
   work local-only and say nothing about the vault.
2. **What goes to the vault.** The vault mirrors output, not memory. Exactly
   three things, all under the vault folder `Chat Workspace/`:
   - `Chat Workspace/Knowledge/<domain>/<file>.md` — copy of every knowledge
     artifact, same name and content as the workspace file, plus a one-line
     source note pointing back to the workspace path.
   - `Chat Workspace/Index.md` — mirror of `knowledge/_index.md`.
   - `Chat Workspace/Sessions/YYYY-MM-DD.md` — one compact note per session,
     written at consolidation: durable takeaways, decisions, open loops. Never
     sync raw transcripts, and never sync `personality.md`, `user.md`, or
     anything under `memory/` — the vault is an output surface, not a second
     memory store.
3. **Track sync state locally.** In `knowledge/_index.md`, a line marked
   `(vault-synced <date>)` means a vault copy exists; unmarked lines are
   local-only and get synced by the next ON session. Backfill markers as you
   sync older artifacts.
4. **Vault etiquette.** Update notes in place rather than recreate; latest-wins
   applies in the vault too; never delete or rename vault notes without an
   explicit user request. If a vault write fails, note the failure in
   `short_term.md` and continue — this workspace remains the source of truth.
5. **Be efficient.** Batch vault writes; locate notes via
   `Chat Workspace/Index.md` or search, never by scanning folders; touch only
   what changed this session.

### Folder hubs & sub-folders (graph conventions)

This is a standing structural preference for how knowledge folders map to graph
nodes — apply it in **both** the vault and the local `knowledge/` tree:

- Every domain folder `Knowledge/<x>/` (and local `knowledge/<x>/`) gets a **hub
  note** `_<name>.md`, tagged **`folder-hub`** (tag = red colour in the graph view's
  color group). All members link up to their hub.
- A **sub-folder** (e.g. `robotics/A`, `robotics/B`) gets its own hub node/REAME,
  tagged **`sub-folder`**. Sub-folder hubs link up to the parent folder hub as well
  as their own members.
- An **interdisciplinary sub-folder** (e.g. `robotics/physical_ai`, spanning both
  AI and robotics) is **kept inside its parent folder** but tagged
  **`interdisciplinary`**, and its hub links to **both** the parent folder hub and
  the other domain hub it spans (here: `ai/_ai` **and** `robotics/_robotics`).
- A folder with sub-folders carries a short **README** (local: a `_index.md` or
  README in that folder; vault: the hub note) describing what lives there and the
  folder's role.

When you create or reorganise folders/sub-folders, follow this convention and note
it as a durable preference.

## Session Workflow

1. **Start of session / before any research** — always fetch the present date first
   (e.g. system clock or a time tool), record it, and confirm the header date is
   current before searching the web or drawing time-sensitive conclusions. Then read
   `personality.md`, `user.md`, `memory/short_term.md`, `memory/long_term.md`.
   Read a topic file only when the conversation touches it.
   Read `knowledge/` files only when relevant — locate them via the domain `_index.md`,
   never by scanning whole folders. If the user references something that should be
   in memory but isn't, say so and ask — do not fabricate continuity. Then check
   whether any `mcp__obsidian__*` tool exists in this session's toolset and set
   vault duty ON or OFF per the section above.
2. **During the session** — behave per `personality.md`. Jot notable facts, decisions,
   and follow-ups into `memory/short_term.md` at natural breaks. When a produced
   artifact has lasting value (explainer, roadmap, comparison, spec), save it under
   the right `knowledge/<domain>/` folder — creating the folder and updating its
   `_index.md` — and link it from the active topic file. If vault duty is ON,
   mirror the artifact into the Obsidian vault per the section above.
3. **End of session / on request ("consolidate")** — run consolidation:
   - Promote durable items from `short_term.md` to `long_term.md` or the relevant
     topic file; resolve contradictions latest-wins; clear `short_term.md` to a
     fresh header.
   - When a thread concludes or a finding proves durable: write/refresh its detail
     file in `memory/long_term_topics/`, keep `long_term.md` to the one-liner +
     link, and mark the active topic file `status: done`.
   - Update `user.md` if any stable fact about the user changed.
   - Append one line to `memory/sessions.md`.
   - If vault duty is ON: write the session note to `Chat Workspace/Sessions/`
     and refresh `Chat Workspace/Index.md`.
4. **Hard rules** — `sessions.md` is append-only; memory files have budgets —
   compress, don't sprawl; mark uncertain facts `(unverified)`; never present
   guessed continuity as real memory; never file session state into `knowledge/`.