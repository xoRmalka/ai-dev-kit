# AI Dev Kit

A generic, stack-agnostic AI development kit for interview take-home tasks and live coding sessions.

## What's In Here

This is the **AI layer** — guardrails and pipeline skills for structured AI-assisted development. No code scaffold. You bring your own stack.

## Pipeline

```
/shape → /propose → /design → /implement → /review → /ship
```

## Quick Start

1. Clone this repo into your project folder (or copy the files into an existing project)
2. Fill in `TASK.md` with the interview task description and constraints
3. Open Claude Code in this directory
4. Run `/shape` to begin

## Files

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Guardrails for Claude Code |
| `AGENTS.md` | Guardrails for Cursor, Copilot, etc. |
| `.cursorrules` | Cursor-specific rules |
| `TASK.md` | Fill in your task brief here |
| `.claude/commands/` | Pipeline slash commands |
| `.ai/` | Pipeline artifacts land here |
| `docs/workflow.md` | Full workflow reference |

## Using with Cursor

The kit includes `.cursorrules` and `AGENTS.md`. Open the project in Cursor and the guardrails are automatically loaded.

## Adapting the Kit

See `docs/workflow.md` for how to add reviewer agents or stack-specific scaffolding.
