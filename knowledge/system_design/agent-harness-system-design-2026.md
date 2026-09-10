---
tags: [folder-hub]
---
# Agent / Coding Harness System Design — Complete Interview Package

> **Topic:** an agentic harness that runs an LLM in a loop with tools (code execution,
> retrieval, browser, shell) — "the dumb scaffold around a smart model" that makes it a
> *reliable, auditable, resumable* product.
> **Audience:** early-career AI/ML + software engineer (Shrey), engineer depth
> **Purpose:** a complete, self-contained package — concepts, a worked architecture,
> trade-offs, evaluation, and interview questions — for system-design interview prep.
> **Date:** 2026-09-10 | **Reference note:** system_design/README.md

---

## 0. One-line summary

An **agent harness** is the runtime that owns the **control loop** — take a user goal,
iterate *model → tool call → observation* until the goal is done or a budget is hit —
while providing everything around it that a raw `while` loop leaves out: **session/state
persistence, tool execution, sandbox isolation, approval gates, progress streaming, and
cost/iteration budgeting**.

The catchphrase worth memorising: *writing the `while` loop is trivial; the harness is
everything that makes a multi-hour autonomous run visible, steerable, interruptible,
and safe.*

---

## 1. Why a harness exists (and the Anthropic rule)

A raw agentic loop (from the Codex deep-dive) is five lines:

```
User
 ↓
Model → Tool Call → Tool Result
 ↑                    │
 └────────────────────┘
```

That demo works. The moment it becomes a **product**, real questions stack up — and they
all live *outside the model*:

- User adds a requirement mid-task → where does the new input go?
- A shell command is running → how do you cancel it?
- Context fills up → fail, or compact and continue?
- The app drops/reconnects → how do you resume the *same* task timeline?
- Shell / file / network / MCP tools → each with what permission bounds?

These are **harness** responsibilities. They decide whether the agent actually gets the
job done. Striking evidence that the harness is no afterthought — **same model, different
harness config** (from Codex on ARC-AGI-3): default 13.3% → with *retained reasoning +
context compaction* 38.3%, using **6× fewer output tokens**. The execution layer moved
results more than the model did.

**The Anthropic rule (say this verbatim):** *start simple — the vast majority of agentic
use cases are better served by a single autonomous loop plus structured tools, NOT by
multi-agent orchestration.* Workflows (deterministic, developer-coded) beat agents
(LLM-decided control flow) for reliability most of the time; only escalate to agents where
workflow flexibility justifies the loss of determinism.

---

## 2. The foundation: agent loop → harness → runtime (the taxonomy)

Frame the evolution the same crisp way the field does (mirror of the Naive/Advanced/Modular
RAG tiers — interviewers recognise the pattern):

1. **The minimal loop** — `while not done: think → call tool → read result → repeat`.
   Good enough for a prototype/demo, collapses on length, interrupts, or context growth.
2. **The harness** — the simplest agent loop *plus* the production concerns: state,
   tools, sandbox, approval, budget, observability. This is what "coding agent harnesses"
   (Codex, Claude Code, and in-box replicas) ship.
3. **The agent runtime** — the harness pushed to *platform* shape: it exposes a
   client-facing protocol (send input, steer, interrupt, subscribe to progress), survives
   disconnects, and is shared by many front-ends (CLI, IDE, desktop, CI, third parties).

**Named control-loop patterns** every interviewer uses as a checkpoint (from the
engineering-handbook agent-architectures catalog):
- **ReAct** — interleave chain-of-thought ("reasoning") with tool actions; the base pattern
  for most tool-using agents.
- **Reflection / Reflexion** — after an attempt, the agent critiques its own output and
  retries with the correction; boosts reliability without a bigger model.
- **Planner–Executor** — a planner (orchestrator) decomposes a goal into a plan and hands
  pieces to one or more executors, repeatedly.
- **Tool-use / function-calling** — the model can call *structured* tools; the "agent loop"
  is the runtime that dispatches and returns results.
- **Multi-agent** — several specialised agents (orchestrator + workers) cooperate. Powerful
  but higher cost/complexity; Anthropic advises *against* as a default.

Keep the taxonomy note: **ReAct is the pattern for a single coding agent; planner-executor
and multi-agent are the escalations you justify, not pick by default.**

---

## 3. The architecture (draw this first)

```
                    ┌─────────────────────────── HARNESS / RUNTIME ────────────────────────────┐
 User goal (CLI/IDE/API)                                                                     │
      │  (App server protocol)                                                               │
      ▼                                                                                       │
 ┌─ CONTROL LOOP (Core Session) ─┐   ┌─ MODEL ─┐   ┌─ MODEL I/O ────────────────┐            │
 │  turn: take input, build prompt│   │  LLM    │   │ system prompt (goal+plan) │            │
 │  while !done && budget:        │   └────────┘   │ scratchpad / thinking       │            │
 │    model → structured tool call│      ▲          └──────────────────────────────┘          │
 │    → dispatch tool             │      │ tool result / observation                          │
 │  emit progress (streaming)     │      └───────────────────────────────────────────────────┘
 └──────────────┬─────────────────┘                │
                │ tool call                        ▼
     ┌──────────▼───────────── TOOL LAYER ─────────────────┐
     │  Tool registry (schema)  →  dispatcher              │
     │   ├─ code exec   ├─ shell/bash  ├─ file read/edit   │
     │   ├─ search/retrieve  ├─ browser    └─ MCP servers   │
     └──────────┬───────────┬───────────────▲──────────────┘
                ▼           │               │
        ┌─ SANDBOX ─┐  ◄────┴─ SAFETY GATE (approval policy)
        │ isolate  │       per-tool allow/deny/ask
        │ secrets  │
        └──────────┘
   ┌────────────  STATE & INFRA  ─────────────┐
   │  memory: scratchpad / working / episodic / long-term   │
   │  budget: iterations, tokens, $, wall-clock            │
   │  observability: telemetry, logs, cost trace            │
   └───────────────────────────────────────────────────────┘
```

**Terminology to say confidently:**
- **Control loop / Core Session** — the stateful loop that owns a task and carries it across
  turns (this is the *engine*, not the API wrapper).
- **Tool interface** — a contract (function schema / MCP) the model calls *structurally*, not
  free text; dispatch is deterministic code, not another LLM call.
- **Tool registry + dispatcher** — registered schemas that the dispatcher maps a tool call to
  the right executor.
- **Sandbox** — the isolation boundary where untrusted tool side-effects (code, shell, file
  writes) run.
- **Safety gate** — the approval policy (permission model) applied *before* a risky action.
- **Observation handling** — turning raw tool output into a bounded, prompt-suitable
  observation (truncation, summarisation) fed back to the model.
- **Budget** — the guardrails (steps/iterations, tokens, cost) that cap a hung run.

---

## 4. Component deep-dive (the parts interviewers drill into)

### 4.1 The control loop & turn model (Codex's Thread / Turn / Item)
An agent task does not fit "one request → one response" REST. A real run is: *user says
"fix the failing test"* → dozens of model outputs, shell commands, file edits, diffs,
approvals, error recoveries. Model that state explicitly:
- **Thread** — the persistable, resumable agent *session* (create, resume, fork, archive,
  subscribe).
- **Turn** — the unit of work *triggered by one user input* (start, **steer**, interrupt,
  wait for completion).
- **Item** — an *atomic* input or output inside a turn (a message, a reasoning block, a
  command, a file edit, a tool call, an approval request) — each with its own started /
  incremental-event / completed lifecycle.

Why this matters (interview gold): the client protocol is **bidirectional** — the client can
`turn/start`, `turn/steer`, `turn/interrupt`; the server pushes *approval requests* for risky
commands/file edits/tool calls. **`turn/steer` carries an `expectedTurnId`** so a late input
can't land in the wrong live turn; **interrupt is not a direct state flip** — it submits an
`Op::Interrupt` to the Core Session and the runtime stops the loop cleanly at a safe point.

### 4.2 The model I/O: prompting, thinking, and context
- The system prompt carries the **goal, plan, and grounding** (retrieved context for RAG-style
  tools, repo map for code agents).
- **Scratchpad / thinking** — the model's intermediate reasoning; keep it out of the final
  observation it sends back unless you want it to persist.
- **Context compaction (retained reasoning)** — when the window fills, *compress rather than
  fail*. Codex's ARC-AGI-3 result (38.3% vs 13.3%, 6× fewer tokens) credits exactly this +
  retained reasoning. This is the biggest single harness lever.

### 4.3 Tool interface & registry (function calling / MCP)
- **Structured function calling** — the model emits a *machine-readable* tool call matching a
  registered schema (name + typed args), not free-text shell. Deterministic dispatch by code.
- **Tool registry** — each tool declares: name, description, JSON schema, permission class,
  timeout, retry policy, observation size cap.
- **MCP (Model Context Protocol)** — the open standard for exposing tools *and* resources/
  prompts to agents. Interviewers increasingly expect you to know it as the converging
  interop layer (Anthropic-originated, industry-adopted). One harness can mount many MCP
  servers as tools.

### 4.4 The tool layer (what a coding-agent harness actually gives the model)
- **code/shell exec** — run the model's code; primary loop for "make it work" benchmarks.
- **file read/edit** — see and modify the repo (read with diff awareness; edit, don't rewrite
  whole files). Edition tools reduce the token cost of emitting whole files.
- **search/retrieve** — codebase search (symbols, references) and external retrieval; this is
  where the RAG package plugs in.
- **browser** — navigate, click, observe the DOM/screenshot for web agents.
- **MCP servers** — arbitrary third-party tools (databases, APIs, internal services).

### 4.5 Sandbox & execution isolation (the safety core)
The highest-risk surface. Untrusted code runs in an **isolated environment**:
- OS/firecracker-style VM or container per task; no access to the host filesystem/network
  except explicitly granted.
- **Secrets** never reach the sandbox directly — injected only via a safe secret manager, and
  redacted in logs/observations.
- **Network egress** controlled; file writes scoped to a workspace dir.
- The sandbox is a **safety-vs-speed trade-off**: full virtualisation is robust but slow to
  boot; container reuse and cached images trade isolation for latency. Indexing/caching
  (pre-warmed images, incremental build) matters for code agents.

### 4.6 Safety gate (approval / permission policy)
Multi-authz, per-tool **permission model** — the key to "autonomous but safe":
- **Scopes**: which tools, which paths, which hosts are allowed.
- **Modes**: *allow* (auto-run), *deny* (blocked entirely), *ask* (human approval required).
- Typical defaults: safe reads/filtered edits auto-run; **destructive, network, or privileged
  shell commands prompt for approval**.
- The approval is a *first-class protocol event*, not a hack — the server raises an approval
  Request and *pauses the loop* until the human answers. This is what makes long-running
  autonomy safe enough to leave unattended.

### 4.7 Memory (working / episodic / long-term)
- **Scratchpad / working** — in-context reasoning & current task state.
- **Episodic memory** — the *trace* of previous turns/observations (persisted so a resumed
  session can recover the timeline).
- **Long-term store** — could be RAG over the repo/docs, an external vector store, or project
  knowledge; retrieval feeds the prompt (link to the RAG package).
- Trade-off: more memory → better continuity but larger context → higher cost/latency; so
  *retrieve, don't dump*.

---

## 5. Advanced harness patterns (name 2–3 in an interview)
- **Context compaction / retained reasoning** — compress history instead of truncating at the
  window limit; the single biggest performance lever (Codex ARC-AGI-3).
- **Self-correction loop** — on test failure or tool error, the agent reads the error, revises,
  and retries (bounded); Reflection/Reflexion.
- **Test-driven agent loops** — the model writes a test *for* the change, runs it, and only
  "done" when green; converts vague generation into verifiable completion.
- **Tool result caching / dedupe** — don't re-run an identical search/command; big latency +
  cost win on loops.
- **Planner-executor decomposition** — break one hard goal into ordered sub-steps; each handled
  separately, results merged.
- **Human-steer mid-run** — let the user correct/redirect the agent between steps without
  restarting the thread (the `steer` primitive).

---

## 6. Scaling & production concerns (interview checkpoint)

They hand you high marks only if you defend *operations*, not just architecture.

- **Cost explosion is the classic agent pitfall.** A loop that spins in a failure/
  self-correction cycle multiplies tokens with no progress. Caps: **max steps/iterations**,
  **token budget per turn and per run**, **cumulative dollar cost**, wall-clock timeout.
  Kill hung runs; surface budget near-exhaustion to the human.
- **State persistence & resume** — persist the Thread (turns + items) so an IDE reconnect or
  process restart resumes the exact timeline. This is why Thread/Turn/Item are persisted
  objects, not local variables.
- **Interrupt semantics** — cancellation must reach the running tool (kill the process), then
  let the loop stop cleanly and persist; never leave a half-applied edit.
- **Observability** — emit progress (each Item's started/events/completed) as a stream so the
  client renders a live timeline; log every prompt, tool call, observation, and cost for
  debugging and audit.
- **Latency & concurrency** — warm the sandbox, cache tool results, parallelise independent
  tool calls, stream tokens; serialise *dependent* steps only.
- **Failure modes / production checklist**
  - **Infinite loop / no-progress spin** → budget + loop-detect (no state change across N steps).
  - **Context overflow** → compaction, not hard fail.
  - **Context pollution** → unbounded tool output flooding the prompt → truncate/summarise
    observations (Observation handling).
  - **Security/privacy** → prompt-injection via retrieved text or tool output; redact secrets,
    sandbox everything, gate destructive ops.
  - **Hung processes** → per-tool timeouts + force-kill on interrupt.

---

## 7. Harness-vs-other-approaches decision helper

Score heuristic to have in your head (model on the RAG-vs-fine-tuning one):

- +3 if the task needs **many sequential tool actions** (multi-step coding, research, browsing).
- +3 if it must **act on live state** (shell, files, browser, APIs).
- +2 if the route to a correct result is **unknowable in advance** (adapt as you go).
- −2 if **determinism** is critical (regulated, reproducible pipelines) → prefer Workflow.
- −2 if **cost/latency** are the binding constraint (agent loops multiply tokens) → prefer a
  single-shot or workflow.
- −1 if a **2–3 step** plan with no branching would solve it.

Score ≥ 5 → agent harness; ≤ −2 → deterministic workflow / single call; in between → hybrid
(agent core with a workflow skeleton). And always start simplest.

---

## 8. The 45-minute interview walk-through (memorise this structure)

1. **Requirements (5–7 min)** — *ask before drawing boxes.*
   - Task: coding agent / browser / research? single-turn or long-horizon? multi-step?
   - Autonomy: fully unattended, or human-in-the-loop? How much approval tolerance?
   - Tools: which (shell, files, code, search, browser, MCP)? untrusted code to run?
   - Scale: users, concurrency, cost cap, latency budget.
   - Safety: secrets, network egress, destructive ops, compliance/audit.
   - Evaluation: how is success measured (tests pass, task-completion, SWE-bench)?
2. **HLD (10–12 min)** — draw the architecture above; name each component with one trade-off.
3. **Deep dive (20–25 min)** — pick the 2 hardest: the control loop/turn model & resume;
   the sandbox + safety gate; context compaction; budget/cost control; tool registry + MCP.
4. **Trade-offs & scale (5–8 min)** — harness vs workflow; cost explosion; failure modes.
5. **Close** — cost per task, scale numbers, monitoring, eval gating.

> Anti-pattern: designing a multi-agent swarm as the knee-jerk default. A focused single-loop
> harness with a named safety + budget story beats an undefended wall of agent boxes.

---

## 9. Example questions (a bank to practise against)

### Warm-up / fundamentals
1. What is an agent harness and why is the loop alone insufficient? (→ §0, §3)
2. What are the ReAct / Reflection / planner-executor patterns, and when do you escalate? (→ §2)
3. The Anthropic rule: when do you choose a *workflow* over an *agent*? (→ §1, §7)

### Control loop & state
4. Walk the lifecycle of one user request through Thread → Turn → Item. Where do you persist it? (→ 4.1)
5. How do you support *mid-run steering* (user adds a requirement) and *interrupt* (cancel a command)? (→ 4.1)
6. Context window fills during a 30-minute run — what do you do? (→ 4.2, 5)
7. How do you recover a session after a process crash / client disconnect? (→ 4.1, 6)

### Tools & execution
8. How do you design the tool interface so the model calls tools reliably? (→ 4.3)
9. What is MCP and why is it converging as the tool standard? (→ 4.3)
10. How do you run *untrusted* code an agent writes, safely? (→ 4.5)
11. A tool returns 10 MB of output — how do you keep the context healthy? (→ observation handling, 6)

### Safety & budget
12. Design the permission model that makes long-running autonomy safe. (→ 4.6)
13. How do you stop an agent from burning your entire bill on a failed loop? (→ 6)
14. How do you prevent prompt-injection from tool output or retrieved text? (→ 4.5, 6)

### Scale & production
15. Many users run coding agents concurrently — what do you scale, and what caps apply? (→ 6)
16. How do you make an agent loop *measurable* for audit and debugging? (→ 6 observability)
17. How would you evaluate a coding agent with no label — tests as the objective? (→ §10)
18. Design a coding-agent harness that passes "fix this failing test" on an unknown repo. (→ §8)

### High-level design prompts
19. "Design an agent harness that plans, runs bash, reads the repo, and verifies with tests."
20. "Design a browser-based assistant agent that can fill forms and extract data."
21. "Design a cost-safe autonomous research agent with human approval for web actions."

---

## 10. Evaluation metrics — say these fluently

Agent/harness evaluation is **multi-tier** — never one number:

- **Task completion / pass rate** — the headline: did the agent finish the task *correctly*?
  - **SWE-bench Verified** — the canonical coding-agent benchmark (real GitHub issues). Literally
    "fix the failing test / land the PR"; score = % of issues resolved. Top harnesses clear ~70–88%
    (e.g. Claude Opus 4.7, ~87.6%, Apr 2026 — quote latest, verify).
  - HumanEval/SWE-agent / AgentBench for general or web agentic competence.
- **Efficiency** — steps to completion, **tokens per task**, cost per task. Same completion at
  6× fewer tokens is a *harness* win, not a model win.
- **Reliability / safety** — unsolicited destructive actions, sandbox escapes, secret leaks,
  permission-model failures. Measure safety-regression, not just efficacy.
- **Operational** — latency, availability, resume success rate, budget-limit hits (a high hit
  rate = the budget is too tight or the loop is spinning).
- **Regression gating** — use a labelled task set + golden trajectories; eval-gate harness
  config changes before rollout (mirror RAGAS-style gating). Reference-free trajectories help
  when labels are scarce.

Quick tip: quote **SWE-bench Verified + tokens-per-task + budget-hit rate** together — that
trio is the strongest "I design harnesses" signal you can give.

---

## 11. Sources & further reading (primary first)

- **OpenAI / Codex — "Codex Harness" architecture deep-dive (2026)** — the App Server →
  Core Session → Agent Loop runtime; Thread/Turn/Item; retained reasoning + context compaction
  ARC-AGI-3 result (13.3% → 38.3%, 6× fewer tokens); approval/interrupt protocol.
  *(primary architecture reference for the control loop, state model, and context lever.)*
- **Anthropic — "Building effective agents" (2024/2025)** — the workflows-vs-agents framing,
  the *start simple* rule, multi-agent caveat, MCP context.
- **Handbook-academy "Engineering Handbook — Agent Architectures" (2026)** — ReAct, Reflection,
  Planning, Tool-Use, Memory, Multi-agent catalog.
- **harness.io — "The Agent Loop Is the New OS" (design philosophy)** — harness as the
  platform/operating concern for autonomous agents. *(name collision with Software-Delivery
  Platform harness.io — cite as the philosophy piece.)*
- **Model Context Protocol (MCP) docs (Anthropic/industry, 2025→)** — the tool/resource/
  prompt interop standard.
- **SWE-bench Verified** — the coding-agent benchmark used across top harness results.
- **RAG package (this folder)** — the retrieval/memory side plugs into an agent harness.

---

*House note: in an interview, say one trade-off per component and one concrete number (e.g.
"compaction took 13.3% → 38.3% ARC-AGI-3 at 6× fewer tokens", "budget: 50 steps / 1M tokens /
$10 per task", "SWE-bench Verified ~87%"). The harness is judged on the *operational* story —
state, safety, budget, observability — not on how cleverly you can write a `while` loop.*