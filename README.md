# Personal Information Vault

A persistent research and learning workspace — a durable knowledge base I keep and
maintain alongside an AI assistant. It combines structured memory (who I am, what I'm
working on, session history) with domain knowledge artifacts (surveys, deep-dives,
architecture notes) that mirror into an Obsidian vault.

## What's in here

```
AGENTS.md                        Operating instructions for the assistant that runs this workspace
personality.md                   Assistant behavior: tone, modes, hard rules
user.md                          Durable facts about me, my interests, active projects

knowledge/                       Reusable content produced by conversations, by domain
├── ai/                          AI surveys & deep-dives (state of AI, safety/eval, architecture, theory)
├── photonics/                   Photonics surveys (state of the field, PsiQuantum)
├── robotics/                    Robotics surveys (state of the field)
└── _index.md                    One-line index of every artifact

memory/                          Workspace memory (not for the vault)
├── short_term.md                Current-session staging notes
├── long_term.md                 Distilled durable knowledge (one line per entry)
├── long_term_topics/            Full detail behind long-term entries
├── topics/                      Active research/learning threads (status, findings, next steps)
└── sessions.md                  Append-only session log
```

## How it works

- **Memory layer** (`memory/`): durable user facts and distilled long-term knowledge
  live in markdown; `short_term.md` is a session staging area that gets consolidated
  on request.
- **Knowledge layer** (`knowledge/`): anything worth re-reading — explainers,
  roadmaps, comparisons, design docs — organized by domain, indexed in `_index.md`.
- **Obsidian sync**: when the assistant's Obsidian MCP bridge is active, knowledge
  artifacts mirror into `Chat Workspace/` in the vault; sync state is tracked as
  `(vault-synced <date>)` markers in `knowledge/_index.md`. Unmarked lines are
  local-only and sync on the next session where the bridge is up.

## Current threads (active as of 2026-09)

- State of **AI** — frontier models, GPT-6 Astra, open models, agents, safety/eval
- **Technical & theoretical architecture** — hybrid transformers, looped transformers,
  reservoir computing, attention adoption trends
- State of **robotics** — humanoids, robot foundation models, warehouse automation
- State of **photonics** — AI/compute, communications, quantum, PsiQuantum

## Contacts / changelog

See `memory/sessions.md` for the append-only session log.