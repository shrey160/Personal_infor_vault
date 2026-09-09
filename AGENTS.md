# AGENTS.md — operating instructions for this chat project

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

## Session Workflow

1. **Start of session** — read `personality.md`, `user.md`, `memory/short_term.md`,
   `memory/long_term.md`. Read a topic file only when the conversation touches it.
   Read `knowledge/` files only when relevant — locate them via the domain `_index.md`,
   never by scanning whole folders. If the user references something that should be
   in memory but isn't, say so and ask — do not fabricate continuity.
2. **During the session** — behave per `personality.md`. Jot notable facts, decisions,
   and follow-ups into `memory/short_term.md` at natural breaks. When a produced
   artifact has lasting value (explainer, roadmap, comparison, spec), save it under
   the right `knowledge/<domain>/` folder — creating the folder and updating its
   `_index.md` — and link it from the active topic file.
3. **End of session / on request ("consolidate")** — run consolidation:
   - Promote durable items from `short_term.md` to `long_term.md` or the relevant
     topic file; resolve contradictions latest-wins; clear `short_term.md` to a
     fresh header.
   - When a thread concludes or a finding proves durable: write/refresh its detail
     file in `memory/long_term_topics/`, keep `long_term.md` to the one-liner +
     link, and mark the active topic file `status: done`.
   - Update `user.md` if any stable fact about the user changed.
   - Append one line to `memory/sessions.md`.
4. **Hard rules** — `sessions.md` is append-only; memory files have budgets —
   compress, don't sprawl; mark uncertain facts `(unverified)`; never present
   guessed continuity as real memory; never file session state into `knowledge/`.