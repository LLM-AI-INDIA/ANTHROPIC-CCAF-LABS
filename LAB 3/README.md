# LAB 3 — Claude Code Project Configuration

This folder holds the system prompt for **Lab 3**, an interactive teaching
flow that walks you through configuring a real project for Claude Code by
wiring up all four of its configuration surfaces around a working
**bookstore MCP server**.

## Contents

| File | Purpose |
|------|---------|
| `LAB3_CLAUDE_CODE_CONFIG_SYSTEM_PROMPT.md` | The full system prompt. Paste it into Claude Code to run the lab. |
| `README.md` | This file. |

## What the lab produces

Running the prompt builds a runnable project folder (default:
`bookstore-claude-config`) containing a read-only SQLite bookstore MCP
server plus the four Claude Code configuration surfaces:

1. **`CLAUDE.md`** — project memory loaded into every session. *Advisory.*
2. **Two slash commands** (`.claude/commands/` → `/seed`, `/query`) —
   reusable prompts you invoke by typing `/<name>`. *Opt-in.*
3. **One hook** (`.claude/settings.json` + `.claude/hooks/auto_smoke.py`) —
   a `PostToolUse` auto-smoke-test that re-runs the smoke test whenever
   `server.py`/`seed_db.py` is edited. *Automatic.*
4. **One MCP server** (`.mcp.json` → `server.py`) — registers the bookstore
   server with Claude Code. *Tool provider.*

The central lesson is the contrast between those behaviors:
**advisory vs. opt-in vs. automatic vs. tool provider.**

## The bookstore MCP server

A FastMCP stdio server over a local read-only SQLite database
(`bookstore.db`: 5 authors, 11 books, 22 sales) exposing:

- **4 tools** — `run_query`, `top_selling_books`, `revenue_by_author`,
  `low_stock_books`
- **2 resources** — `bookstore://tables`, `bookstore://schema/{table}`
- **1 prompt** — `data_analyst`

Read-only is enforced at the storage layer via
`sqlite3.connect("file:...?mode=ro", uri=True)` — never by SQL-keyword
filtering.

## How to run the lab

1. Open Claude Code.
2. Paste the contents of `LAB3_CLAUDE_CODE_CONFIG_SYSTEM_PROMPT.md` as the
   system prompt (or reference the file).
3. Follow the interactive flow: pick a target folder, confirm the file-tree
   preview, let it generate the base files and create the venv, then write
   the slash commands and the hook, build the database yourself with
   `/seed`, and verify with the smoke test.

## Requirements

- Python 3.10 or newer
- No API key, no paid services, no runtime internet dependency (one
  `pip install mcp[cli]` aside)
