# AI Dev Kit — Design Spec
*Date: 2026-06-22*

## Purpose

A generic, stack-agnostic AI development kit for interview take-home tasks and live coding sessions. Focused entirely on the AI layer: guardrails, pipeline skills, and artifact management. No code scaffold — you bring your own stack.

---

## Pipeline

```
/shape → /propose → /design → /implement → /review → /ship
```

Each stage is a slash command in `.claude/commands/`. Each stage has a clear input, output, and gate before the next stage begins.

---

## Repo Structure

```
ai-dev-kit/
├── CLAUDE.md                    # Claude Code guardrails (loaded every session)
├── AGENTS.md                    # Shared guardrails (Cursor, Copilot, etc.)
├── .cursorrules                 # Cursor-specific overrides
├── TASK.md                      # Fill in at start: task brief, constraints, time limit
├── .claude/
│   ├── settings.json            # Hook: warn if design.md missing before /implement
│   └── commands/
│       ├── shape.md             # Explore, no files written
│       ├── propose.md           # Write .ai/proposal.md → auto-commit on approval
│       ├── design.md            # Write .ai/design.md → auto-commit on approval
│       ├── implement.md         # Code against .ai/design.md
│       ├── review.md            # Check code vs proposal + design, flag gaps
│       └── ship.md              # Final checklist, verify artifacts + README
├── .ai/
│   ├── .gitkeep
│   ├── proposal.md              # Gate 1 artifact (created by /propose)
│   └── design.md                # Gate 2 artifact (created by /design)
├── docs/
│   └── workflow.md              # How to use the kit, how to adapt it
└── README.md
```

---

## Guardrails (CLAUDE.md / AGENTS.md)

Core rules enforced across all stages:

1. **Plan before code** — never write implementation code without an approved `design.md`
2. **TASK.md is the source of truth** — all skills read it; never invent or assume requirements
3. **`/shape` is chat only** — no files written, no code, no proposals; exploration only
4. **Gate commits** — `/propose` auto-commits `proposal.md` on approval; `/design` auto-commits `design.md` on approval. Git history reflects the pipeline.
5. **Interview mode** — code must be readable and explainable out loud. No over-engineering. No premature abstractions.
6. **One stage at a time** — don't skip ahead. Each gate must be explicitly approved before proceeding.

`.cursorrules` mirrors these rules with Cursor-specific behavior: respect artifact files, don't auto-complete past a gate, treat `TASK.md` as the task brief.

---

## Skill Contracts

| Command | Reads | Writes | Gate condition |
|---|---|---|---|
| `/shape` | `TASK.md` | nothing | User says "ready to propose" |
| `/propose` | `TASK.md` | `.ai/proposal.md` + git commit | User approves proposal |
| `/design` | `TASK.md`, `.ai/proposal.md` | `.ai/design.md` + git commit | User approves design |
| `/implement` | `.ai/design.md`, `.ai/proposal.md` | code | — |
| `/review` | `.ai/proposal.md`, `.ai/design.md`, code | review report in chat | — |
| `/ship` | everything | final commit + submission summary | — |

---

## Skill Behaviors

### `/shape`
- Read `TASK.md` at start
- Ask clarifying questions to build a mental model
- Surface ambiguities and constraints
- Never write files, never propose solutions
- End when user says "ready to propose"

### `/propose`
- Read `TASK.md`
- Write `.ai/proposal.md` with: problem restatement, constraints, chosen approach (1 of 2-3 options considered), out-of-scope decisions
- Present to user, wait for approval
- On approval: git commit with message `feat: proposal approved`
- Hard stop — do not proceed to design without explicit approval

### `/design`
- Read `TASK.md` and `.ai/proposal.md`
- Write `.ai/design.md` with: components, data flow, API shape / schema, tech decisions, key trade-offs
- Present to user, wait for approval
- On approval: git commit with message `feat: design approved`
- Hard stop — do not proceed to implement without explicit approval

### `/implement`
- Check `.ai/design.md` exists — if not, refuse and direct to `/design`
- Read `proposal.md` and `design.md` before writing any code
- Follow design exactly; surface conflicts rather than silently deviating
- Commit code in logical chunks

### `/review`
- Check implementation against `.ai/proposal.md` (did we build what we said?) and `.ai/design.md` (did we build it how we said?)
- Flag: missing requirements, deviations from design, code that would be hard to explain in an interview, missing edge cases
- Output: chat report only (no file written)

### `/ship`
- Verify all artifacts exist: `TASK.md`, `.ai/proposal.md`, `.ai/design.md`
- Verify README explains how to run the project
- Final git commit with message `feat: ship — task complete`
- Print submission summary: what was built, what was out of scope, how to run it

---

## Hook: Pre-implement Gate

`.claude/settings.json` contains a hook that runs before the AI writes any code files. It checks whether `.ai/design.md` exists and is non-empty. If not, it emits a warning reminding the user to complete `/design` first.

---

## Artifacts

All pipeline artifacts live in `.ai/`. This folder is committed to git so the pipeline history is visible.

- `.ai/proposal.md` — Gate 1. Problem + approach.
- `.ai/design.md` — Gate 2. Technical design.

`TASK.md` lives at root (not in `.ai/`) because it's the task brief you fill in before the pipeline starts — it's input, not an artifact.

---

## How to Use

1. Clone the kit into a new project folder
2. Fill in `TASK.md` with the interview task description and constraints
3. Run `/shape` to explore
4. Run `/propose` → approve → run `/design` → approve → run `/implement`
5. Run `/review` when done coding
6. Run `/ship` to finalize

---

## Out of Scope

- Code scaffold (stack-agnostic — you bring your own)
- CI/CD or deployment
- Multi-agent orchestration (single AI session per task)
- Python support (TypeScript/Node.js primary, but kit works with any language)
