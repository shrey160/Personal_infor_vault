# knowledge/system_design/ — System Design

Domain folder for **system-design** study material, kept at engineer-depth for Shrey
(same house style as ai/, robotics/, space/, photonics/). Topics so far:
**Retrieval-Augmented Generation (RAG)** and the **agent/coding harness**.

## What lives here
Interview-prep packages for AI-adjacent system design. "Complete package" = the
concepts + a worked architecture + the trade-offs + evaluation + example interview
questions — everything needed to walk into the room cold.

## Files
- `rag-system-design-2026.md` — the full RAG package: foundations, two-phase
  architecture (ingestion + query), per-component deep-dives (chunking, embedding,
  vector DB, hybrid search, reranking, prompt, eval), Naive/Advanced/Modular
  taxonomy, scalability & cost trade-offs, RAG-vs-fine-tuning, failure modes, a
  45-minute interview walk-through, and a question bank.
- `agent-harness-system-design-2026.md` — the full **agent/coding-harness** package:
  the control loop (Thread/Turn/Item), tool interface + registry (function calling /
  MCP), sandbox + safety-gate (approval policy), memory tiers, context compaction,
  cost/budget control, error modes, harness-vs-workflow decision helper, a 45-minute
  interview walk-through, and an evaluation section (SWE-bench Verified).
- `README.md` — this hub / folder role.

## Cross-links
- `ai/` — LLM internals & inference (`state-of-ai-2026.md`,
  `ai-technical-architectural-2026.md`); RAG sits on top of the LLM inference
  layer Shrey already builds at nanoGPT scale; the harness runs that inference in
  a tool loop.
- `dshtools/` — the Obsidian MCP tooling the vault mirror runs on (MCP is also the
  harness tool interop layer).

## Sources (primary first)
The RAG package is grounded in the original **RAG paper (Lewis et al., NeurIPS
2020)**, the **Gao et al. RAG survey taxonomy** (Naive/Advanced/Modular), the
**PracHub RAG-at-scale interview guide**, and a **FAANG-style RAG system-design
interview codex**. The harness package is grounded in the **OpenAI/Codex harness
architecture** (Thread/Turn/Item, context compaction), **Anthropic "Building
effective agents"** (workflows-vs-agents, start simple), the **handbook-academy
agent-architectures catalog** (ReAct/Reflection/Planner), **MCP**, and **SWE-bench
Verified**. See the Sources section in each note.

_Structural note (AGENTS.md convention): each folder carries a README as its hub
(`folder-hub` in the vault graph). Vault mirror lives under
`Chat Workspace/Knowledge/system_design/`._