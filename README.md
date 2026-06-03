# ANTHROPIC-CCAF-LABS

**Claude Code Agentic Framework — hands-on labs**

A set of five self-contained labs that turn **Claude Code** into an
interactive *project generator and configurator*. Each lab is driven by a
single system-prompt Markdown file: you paste it into Claude Code, answer a
short guided flow, and walk away with a real, runnable artifact — a Colab
notebook, an MCP server, a configured project, or a tested agent.

The labs build on the same teaching philosophy: **lock the architectural
invariants in the prompt, leave the implementation to the AI author**, and
narrate every step so the learner understands not just *that* it works but
*how* and *why*.

---

## Repository layout

```
ANTHROPIC-CCAF-LABS/
├── LAB 1/   Multi-Agent Project Generator (Colab Edition)
├── LAB 2/   Bookstore MCP Server
├── LAB 3/   Claude Code Project Configuration
├── LAB 4/   AI Medical Report Analyzer
└── LAB 5/   Support Agent v2 — Stress-Tested AI Customer Support
```

Each lab folder contains a system-prompt `.md` (the spec that drives Claude
Code) and, in most cases, a `README.md`. Some labs also ship sample inputs.

---

## The labs

### LAB 1 — Multi-Agent Project Generator (Colab Edition)
Turns Claude Code into a project designer that generates **single-file
Google Colab notebooks** implementing multi-agent systems on the **Claude
Agent SDK**. You give a problem statement; the system designs a solution
with **one orchestrator + 2–5 specialized subagents** dispatched in
parallel via a single `Task` message, each isolated in its own context,
then stitches their outputs into a versioned `report.md`.

- **Teaches:** orchestrator-as-dispatcher, subagent isolation and OUTPUT
  FORMAT contracts, single-message parallel dispatch, bounded tools as a
  security boundary, Colab-native async.
- **Tech:** Claude Agent SDK, `nest-asyncio`, tools restricted to
  `["Task", "Write"]`, `acceptEdits` permission mode.
- **Deliverable:** one valid nbformat-4 `.ipynb` that runs entirely in
  Colab under the user's account (Claude never executes it).
- **Default use case:** a Meeting Action Agent (transcript → decisions,
  action items, follow-up emails).

### LAB 2 — Bookstore MCP Server
Guides you through building a fully functional **Model Context Protocol
(MCP) server** in Python that exposes a local SQLite bookstore database to
Claude Desktop and Claude Code.

- **Teaches:** the three MCP primitives (**tools, resources, prompts**),
  production patterns like storage-layer read-only enforcement, and proper
  environment configuration.
- **Tech:** Python, **FastMCP** decorators, SQLite, JSON-RPC over stdio.
- **Deliverable:** a complete, seeded, and smoke-tested MCP server project.

### LAB 3 — Claude Code Project Configuration
An interactive flow that wires up **all four Claude Code configuration
surfaces** around the bookstore MCP server from Lab 2 (carried
self-contained in the prompt).

- **The four surfaces:** `CLAUDE.md` (advisory memory), slash commands in
  `.claude/commands/` (opt-in), a `PostToolUse` hook in
  `.claude/settings.json` (automatic), and `.mcp.json` (tool provider).
- **Teaches:** the distinction between **advisory vs. opt-in vs. automatic
  vs. tool-provider** — especially opt-in (a command runs only when typed)
  vs. automatic (a hook fires on its own event).
- **Deliverable:** a complete, runnable project folder with the MCP server
  plus all four configuration files, an auto-smoke-test hook, and `/seed`
  and `/report` commands.

### LAB 4 — AI Medical Report Analyzer
A **tool-use + retry-validation** system that converts unstructured medical
report PDFs into validated structured JSON.

- **Teaches:** **forced tool use** (guarantees JSON-shaped responses),
  **Pydantic** schema enforcement (catches type/range failures), and a
  **retry loop** that feeds labelled error patterns back to the model for
  informed re-extraction (up to 3 attempts).
- **Tech:** Anthropic SDK, `anthropic`, `pydantic`, `pdfplumber`,
  `python-dotenv`. Python 3.10+.
- **Deliverable:** either a local project folder (`main.py`, schema,
  extractor, validator, logger, retry handler, PDF reader, sample, guides)
  **or** a single self-contained Colab notebook — both producing
  timestamped JSON plus a human-readable summary.

### LAB 5 — Support Agent v2 (Stress-Tested AI Customer Support)
Builds a resilient, multi-turn AI **customer-support agent** and
systematically stress-tests its recovery against simulated failures.

- **Teaches:** production-grade error handling and **fault-injection
  methodology** — API errors, tool timeouts, context overflow — paired with
  automatic resilience mechanisms.
- **Tech:** Anthropic Claude API, Python 3.10+, a fault-injection
  framework, structured logging, JSON reporting.
- **Deliverable:** a working support agent (order lookups, refunds,
  tracking) with a **45-test stress suite** (5 scenarios × 9 fault
  configs). Like Lab 4, you choose one of two variants — a **local Python
  project** (CLI-driven via `run_stress_tests.py`, report written to
  `stress_test_report.json`) **or** a single self-contained
  `support_agent_v2_colab.ipynb` **Colab notebook**.

---

## Common patterns across the labs

- **System-prompt-driven.** Each lab is a single `.md` spec that locks
  invariants (architecture, tool sets, file manifests) while leaving naming
  and implementation to Claude.
- **Guided ~10-step flow.** Briefing → variant/model selection → target
  folder (with auto-versioning so prior runs are never overwritten) →
  generation → run/verify → next-step menu.
- **Structured choosers, not free text**, for fixed decisions; free text
  only where the learner supplies their own content.
- **Narrate, then act.** The model explains what each step does and why
  before doing it.
- **Hand-offs the learner owns.** Where a step requires the user's runtime
  (running a Colab notebook, seeding a database, supplying an API key),
  Claude hands it off rather than doing it.

---

## Requirements

Varies by lab, but broadly:

- **Claude Code CLI** installed (drives every lab).
- **Python 3.10+** for the local labs (2, 3, 4, 5).
- **An Anthropic API key** (`sk-ant-…`) for labs that call the API/SDK
  (1, 4, 5).
- **A Google account** if you pick a Colab variant — Lab 1 (Colab-only),
  or the Colab option of Labs 4 and 5.
- No paid services beyond Anthropic API usage; the MCP/config labs (2, 3)
  need no API key at runtime.

---

## How to run a lab

1. Open the lab folder and read its `README.md`.
2. Copy the lab's system-prompt `.md` into Claude Code (or reference it).
3. Follow the interactive flow — pick a target folder, confirm previews,
   and let Claude generate the project.
4. Complete the hands-on steps the lab hands to you (run the notebook in
   Colab, seed the database, supply a PDF or API key, etc.).

---

## Suggested order

Take the labs in numbered order:

1. **LAB 1** — multi-agent orchestration with the Claude Agent SDK.
2. **LAB 2** — build the bookstore MCP server (foundation).
3. **LAB 3** — configure Claude Code around that server (the four surfaces).
4. **LAB 4** — forced tool use + validation + retry (structured extraction).
5. **LAB 5** — resilience and stress-testing for production agents.

In particular, do **Lab 2 before Lab 3** — Lab 3 configures Claude Code
around the bookstore MCP server that Lab 2 builds, so taking them back to
back keeps that thread intact.

---
