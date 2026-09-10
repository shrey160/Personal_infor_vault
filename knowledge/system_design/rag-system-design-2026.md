---
tags: [folder-hub]
---
# RAG System Design — Complete Interview Package

> **Topic:** Retrieval-Augmented Generation (RAG) architecture
> **Audience:** early-career AI/ML + software engineer (Shrey), engineer depth
> **Purpose:** a complete, self-contained package — concepts, a worked design,
> trade-offs, evaluation, and interview questions — for system-design interview prep.
> **Date:** 2026-09-10 | **Reference note:** system_design/README.md

---

## 0. One-line summary

RAG answers questions about a **private, changing, and large** corpus by (1) **ingesting**
and indexing that corpus so it can be searched, then (2) **retrieving** the most relevant
chunks for each query and **generating** a grounded answer from them — instead of relying on
the model's frozen training knowledge.

---

## 1. Why RAG exists (and when NOT to use it)

LLMs know what they learned at training time (static, stale), and their index doubles as
their "memory" (hallucination, no citations). RAG injects **ground truth from outside the
weights** at inference time.

**When RAG wins** (the classic decision criteria):
- Need **citations / source grounding** (compliance, enterprise QA, customer support).
- Data **updates frequently** — add a doc, and it answers within hours; no retraining.
- **Large or heterogeneous knowledge base** you could never fit in a context window.
- **Factual accuracy is critical** and hallucination is unacceptable.

**When RAG is the wrong tool:**
- The task is about **learned behavior/style**, not facts (tone of the answer, code style) → fine-tune.
- **Latency is the binding constraint** — retrieval adds a round-trip before generation.
- The dataset is **small and static** — fine-tuning is cheaper in the long run.

**Core trade-off table (memorize for interviews):**

| Criterion | RAG | Fine-tuning |
|---|---|---|
| Freshness | Real-time (add docs instantly) | Retraining required |
| Grounding / citations | Strong (traceable sources) | Weak (black box) |
| Latency | Slower (retrieve + generate) | Faster (generate only) |
| Operational cost | Higher per query (retrieval + LLM) | Lower per query, higher training |
| Need for internal data | Low | High (your data, in your loops) |
| Interpretability | High (inspect sources) | Low |
| Use when | Factual QA, internal docs, support | Chatbot voice/style, code-gen fmt |

Rule of thumb used in interviews: **RAG for facts + freshness + citations; fine-tuning
for behavior/style; hybrid when you want both.**

---

## 2. The foundation: Naive → Advanced → Modular (the Gao taxonomy)

This is the standard way the field frames RAG evolution, and interviewers use it as a
checkpoint. Keep the three tiers crisp.

1. **Naive RAG** — the vanilla pipeline: ingest → index → retrieve → generate. Weaknesses:
   poor retrieval precision, hallucinations, redundant/fragmented chunks, no
   anti-model-garbage handling.
2. **Advanced RAG** — engineering re-starts: **pre-retrieval** optimisation (query
   rewriting, chunk-optimisation, routing), **post-retrieval** optimisation (reranking,
   compression/context pruning), and Light/modular retrieval strategies (hybrid search).
   This is what the FAANG-style design usually lands on.
3. **Modular RAG** — a super-scoped, configurable system where components are swapped
   (search, memory, routing, prediction). The reference: **Gao et al., "Retrieval-Augmented
   Generation for Large Language Models: A Survey"** (2023) — the paper most interviewers
   will be thinking of.
4. **(Optional fourth spoke)** the **original RAG paper**: Lewis et al., NeurIPS 2020 —
   first showed augmenting a seq2seq generator with a non-parametric memory achieves
   SoTA on knowledge-intensive NLP (NQ, etc.). Naming this paper earns credibility.

---

## 3. The two-phase architecture (draw this first)

Every RAG system is **two distinct phases**:

```
┌─ INGESTION (offline / async, batch) ──────────────┬─ RETRIEVE & GENERATE (online / sync) ─┐
│  Documents (PDF, wiki, code, DB)                  │  User query → Query rewrite            │
│    → parse/OCR                                    │  → embed with SAME model as ingest     │
│    → chunk (best with overlap / semantics)        │  → hybrid vector + BM25 search (top K) │
│    → embed (dense vector)                         │  → RRF fuse                            │
│    → store in vector DB (+ BM25 + metadata)       │  → cross-encoder rerank → top-k        │
└───────────────────────────────────────────────────┴── prompt(query+chunks) → LLM → answer/cite │
```

**Terminology to say confidently:**
- **Ingestion pipeline** = parse, chunk, embed, index, metadata. Runs async (batch / scheduled).
- **Query path** = embed query → search → rerank → build prompt → generate → cite → stream.
- **Embedding model MUST match** between ingestion and query — a mismatch silently breaks retrieval.

---

## 4. Component deep-dive (the parts interviewers drill into)

### 4.1 Chunking — "the single most important decision"
- **Fixed-size splits are amateur** — they sever semantic units mid-sentence and lose context.
- Prefer **structure-aware** chunking: split on headers/sections/paragraphs (markdown headings,
  HTML tags).
- Add **overlap** (50–100 tokens) so a fact spanning a boundary isn't lost.
- **Semantic chunking** — group by embedding similarity / discourse units — best retrieval quality.
- **Hierarchical (parent-child)** — retrieve a small *child* chunk for precision, return its
  large *parent* chunk to the LLM for context. "Retrieve small, return big."
- Chunk size is a **use-case** lean — coarse for summarisation/writing, fine for factual lookup.

### 4.2 Embedding models
- Map text → high-dim vector (e.g. **OpenAI `text-embedding-3-large`** 1536-d, or open-source
  **`all-MiniLM-L6-v2`**, **`bge-*`**, **`sentence-transformers`**).
- **Match the model at ingest and query.** Batch-embed during ingestion to amortise cost.
- The embedding is a **bi-encoder** (encodes each text independently).

### 4.3 Vector database (index)
- Built for **semantic similarity search**, typically via **HNSW** (Hierarchical Navigable
  Small World) for fast approximate nearest-neighbour (ANN).
- Vendors/shapes: **Pinecone, Milvus, Weaviate, Qdrant**, or self-hosted **FAISS**, or a
  hosted platform search like **Azure AI Search**.
- Keep **metadata separate** (e.g. PostgreSQL) for filtering by source/dates/org, plus a
  **sparse/keyword index** (Elasticsearch) for hybrid search.

### 4.4 Hybrid search (dense + sparse) — near-must-have
- **Dense (vector)** = semantic ("PTO" ⇄ "vacation days"); **sparse (BM25)** = exact keyword
  (product IDs, acronyms, proper nouns). Vector alone misses exact tokens; BM25 alone misses
  meaning. So **hybrid**.
- **Fuse with Reciprocal Rank Fusion (RRF)**: score(doc) = Σ₁ 1/(k + rank), k≈60. Dense + sparse
  rankings merged. This is the 2026 gold standard.

### 4.5 Reranking — the precision lever
- Bi-encoder retrieval is **fast but approximate**; a **cross-encoder** (e.g. ms-marco-MiniLM)
  scores each (query, chunk) pair directly — **slow but accurate**.
- Standard pattern: **retrieve 20–40 with bi-encoder → rerank to final top 5–10 with
  cross-encoder**. Cost: an extra pass, big precision win (commonly +10–15%).

### 4.6 Query transformation
- **Don't search with the raw query.** Rewrite/expand it first:
  - Expand acronyms / add context (e.g. from a previous turn).
  - **Multi-query / HyDE** — generate a hypothetical answer document, embed *that* to close
    the gap between question-space and answer-space.
  - Multi-turn: carry conversation history into the rewrite, not just the last turn.
- **Routing** — route simple lookups vs complex reasoning to different retrieval/LLM paths.

### 4.7 Prompt & grounded generation
- Template: system instruction + conversation history + **[Source N] (src)** chunks + question.
- Key instructions: "Answer using ONLY the context"; "if not in context, say you don't know";
  "cite [Source N]"; **temperature ~0.0** for factual answers.
- **Citations**: map each generated [Source N] back to its source doc — this is both UX and
  hallucination control. Cites are either auto-extracted from prompts or the LLM emits them.
- **Stream** via SSE (Server-Sent Events) so time-to-first-token feels fast (UX).

---

## 5. Advanced RAG techniques (name 2–3 in an interview)
- **Query rewriting/HyDE/multi-query** (above).
- **Context pruning / prompt compression** — drop redundant chunks to cut tokens + latency.
- **Metadata / pre-filtering** — date range, document type, org filter *before* similarity.
- **Caching** — dedupe + cache frequent queries (Redis) for latency & cost.
- **Post-retrieval dedup & de-duplicate** overlapping chunks.
- **Small-to-big / parent-doc** retrieval.
- **Graph RAG / knowledge-graph** augmentation for multi-hop questions.

---

## 6. Scaling & production concerns (interview checkpoint)

They only hand you high marks in senior interviews if you defend **how it scales**.

- **Latency:** target end-to-end < 5s. Budget exemplar: query rewrite ~0.1s, retrieval ~0.2s,
  rerank ~0.3s, generation 3–4s. **Stream** generation. Add per-stage latency telemetry.
- **Backpressure & retries:** LLM + vector DB calls have rate limits; queue ingestion; retry
  with exponential backoff on transient failures; graceful degradation under 10× spikes.
- **Cost:** the big three = **embedding generation, vector storage, LLM inference**.
  Batch embeddings, route simple queries to a cheaper model, keep context lean.
- **Data freshness:** incremental + scheduled re-index; docs visible within 24h; do a
  *change-indexing* (only re-embed changed docs).
- **Vector-DB + HNSW trade-offs:** recall vs latency — tune `efConstruction`/`efSearch`,
  index memory (RAM-bound at scale), replica vs shard for QPS.
- **Operability:** logging every query+chunks+answer for debugging/audit; dashboards for RAGAS
  deltas; A/B-test retrieval/chunking/prompt changes; **eval gating** before rollout.

### RAG failure modes / when you have a production checklist
- **Retrieval failure** → relevant chunk not retrieved → answer (may still answer from
  training / hallucinate). Guard with eval (context recall) + hybrid/rerank.
- **Chunk bloat / ordering** → context overflow or poor answer; keep chunk-to-context budget.
- **Stale content** → wrong fresh answers; watch re-index jobs.
- **Privacy/security** → PII, prompt-injection via retrieved docs; sanitise, filter.

---

## 7. RAG-vs-fine-tuning decision helper (put these numbers in your head)
A scoring heuristic interviewers like:
- +3 if it needs citations, +3 if data updates often, +2 if facts critical, +2 if large corpus.
- −2 if latency-critical, −2 if needs custom style, −1 if small static data.
- Score ≥ 5 → RAG; ≤ −3 → fine-tune; else hybrid.

---

## 8. The 45-minute interview walk-through (memorise this structure)

Use this exact progression (taken from FAANG-style codex practice):

1. **Requirements (5–7 min)** — *ask before drawing boxes.*
   - Data: type (PDF/wiki/code/DB?)? count (100K?)? update frequency? structure (multi-modal)?
   - Query: complexity? QPS? multi-turn? conversational?
   - Quality: how bad must hallucinations be? citations required? latency budget? languages?
   - Constraints: LLM choice, cloud/on-prem, cost, privacy/security.
   - Evaluation: how is success measured? A/B?
   - *Restate*: functional + non-functional + key challenges.
2. **HLD (10–12 min)** — draw the two-phase box above; name the components; give one trade-off
   per box (don't paint the whole diagram with a single filler sentence).
3. **Deep dive (20–25 min)** — pick the 2 hardest: chunking, hybrid+rerank, prompt/citations,
   evaluation.
4. **Trade-offs & scale (5–8 min)** — RAG-vs-fine-tuning, latency/cost table, failure modes.
5. **Close** — cost per query, scale numbers, monitoring.

> Anti-pattern to avoid: drawing a crowded diagram with no reasoning. A focused design with a
> named bottleneck beats a wall of boxes with nothing defended.

---

## 9. Example questions (a bank to practise against)

### Warm-up / fundamentals
1. What is RAG and when would you choose it over fine-tuning? (→ §1, §7)
2. Name the two phases of a RAG system and the data flow between them.
3. Why must the embedding model match between ingestion and query? (→ 4.2)

### Data & chunking
4. How do you chunk a 50-page PDF with tables and figures? Justify the strategy.
5. What's the downside of fixed-size chunking? What do you do instead? (→ 4.1)
6. How do you handle source freshness — a doc is deleted, a doc is edited mid-day?
7. How would you ingest code, PDFs, *and* structured DB rows into one corpus?

### Retrieval
8. Why is pure vector search insufficient for a product-ID / legal-term query? (→ 4.4)
9. Explain dense vs sparse retrieval and how RRF fuses them.
10. A query returns 40 chunks. Which 5 do you send to the LLM, and why? (→ 4.5 rerank)
11. How do you handle a *multi-turn* "what did you just say?" question in RAG?
12. What is HyDE / multi-query generation and when would you use it? (→ 4.6)

### Generation & quality
13. How do you stop the LLM from answering from its pretrained knowledge instead of your docs?
14. How do you force citations and verify they're accurate? (→ 4.7, 5)
15. How would you make a RAG answer *hallucination-free*? (push past "it can't hallucinate").

### Scale & production
16. User queries spike 10× — what breaks? How do you survive? (→ 6)
17. How do you optimise cost on embeddings, storage, and LLM inference?
18. How do you evaluate a RAG system with no labeled data? (→ §10 RAGAS)
19. Design a RAG system for 100M docs and 1K QPS. What's your index strategy?

### High-level design prompts (from the PracHub/FAANG sources)
20. "Design a RAG Q&A over an internal corpus with citations." Work the 5-step structure.
21. "Design a customer-support agent grounded in your product docs, with chat history."
22. "Design a fresh-knowledge assistant for a fast-moving industry (e.g. a legal tracker)."

---

## 10. Evaluation metrics (RAGAS) — say these fluently

The standard set (RAGAS framework — name it):
- **Faithfulness** — is the answer *grounded* in the context, with no fabricated claims?
- **Answer relevancy** — does the answer actually address the question?
- **Context precision** — are the *retrieved* chunks relevant? (signal: noise ratio)
- **Context recall** — did we retrieve *everything* needed for a good answer?

Vision targets often quoted: faithfulness >0.9, answer-relevancy >0.85, context precision/recall
high. Add operational metrics: latency, cost, citation-accuracy, user-satisfaction.

For a labelled test set, evaluate per-question; when you can't hand-build labels, use
**reference-free** evaluations (TruLens-Eval, ARES) — important when you're new to RAG
and have no benchmark corpus.

---

## 11. Sources & further reading (primary first)

- **Lewis et al., "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks", NeurIPS
  2020** — the original RAG paper.
- **Gao et al., "Retrieval-Augmented Generation for Large Language Models: A Survey" (2023)** —
  the Naive/Advanced/Modular taxonomy and techniques map.
- **PracHub, "Designing RAG Architecture at Scale: An AI Engineering Interview Guide" (2026)** —
  ingestion + retrieval/generation, chunking, hybrid search/RRF, scaling trade-offs.
- **FAANG-style RAG system design interview codex** (girijesh-ai/ai-interview-codex) — the full
  45-min structure, chunking/Hybrid/rerank/prompt/RAGAS code, RAG-vs-fine-tuning.
- **Microsoft Learn, "Build Advanced Retrieval-Augmented Generation Systems"** &
  **Azure AI Search RAG overview** — vendor-agnostic advanced RAG patterns.
- **IBM / Google Cloud "What is RAG"** — definitions.

---

*House note: write/down the decision tables, the 5-step structure, and 3–4 technique names as
flashcards. In an interview say one trade-off per component and one concrete number (e.g.
"rerank to top-5", "RRF k≈60") — that's what separates a memorised answer from a designed one.*