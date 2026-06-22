# AI Dev Kit — Workflow Reference

## Pipeline

```
/shape → /propose → /design → /implement → /review → /ship
```

## How to Use This Kit

### Starting a new interview task

1. Clone this repo (or copy all files into your project folder)
2. Open `TASK.md` and fill in the task description, constraints, and success criteria
3. Open Claude Code in this directory
4. Run `/shape` to begin

### Stage by stage

**`/shape`** — Exploration only. Chat with the AI to build a shared understanding. Ask clarifying questions. No files are written. End when you feel confident about the problem and say "ready to propose."

**`/propose`** — The AI writes `.ai/proposal.md` (problem restatement + chosen approach). Review it carefully. The proposal locks in *what* you're building. Approve to trigger a git commit and move to design.

**`/design`** — The AI writes `.ai/design.md` (components, data flow, interfaces, tech decisions). Review it carefully. The design locks in *how* you're building it. Approve to trigger a git commit and move to implementation.

**`/implement`** — The AI writes code based on `design.md`. Iterate freely. The AI will surface any conflicts with the design rather than silently deviating.

**`/review`** — The AI reviews the implementation against the proposal and design. Chat output only — no files written. Use this before `/ship` to catch gaps.

**`/ship`** — Final checklist (artifacts present, README complete, no uncommitted changes) + final git commit + submission summary.

## Artifacts

| File | Created by | Purpose |
|------|-----------|---------|
| `TASK.md` | You | Task brief and constraints — fill in at start |
| `.ai/proposal.md` | `/propose` | Gate 1: problem + chosen approach |
| `.ai/design.md` | `/design` | Gate 2: technical design |

## Git History After a Complete Task

```
feat: ship — task complete
[implementation commits]
feat: design approved
feat: proposal approved
feat: initial task setup
```

This history shows structured thinking — useful to walk through in a debrief.

## Using With Cursor

`AGENTS.md` and `.cursorrules` load automatically when you open the project in Cursor. The same pipeline rules apply. Cursor will not write code before `design.md` exists.

## Adapting the Kit

### Add a specialist reviewer

Create `.claude/commands/review-security.md` (or `review-architecture.md`, etc.) with a focused review prompt for that dimension. Run it between `/review` and `/ship`.

### Add a stack-specific scaffold

Create `.claude/commands/scaffold.md` that sets up your preferred stack (e.g. "Create a Next.js app with TypeScript, Tailwind, and Prisma"). Run it after `/design` is approved, before `/implement`.

### Change where artifacts live

Update the paths in each command file. The `.ai/` folder is a convention, not a requirement.

### Use with a team

Add reviewer agents (security, architecture, UX) as separate commands. Each reads `design.md` and the code, produces a report. Gate `/ship` on all reviewers passing.
