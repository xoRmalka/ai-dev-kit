Before writing any code, run these checks:

1. Check `.ai/design.md` exists and is non-empty:
   ```bash
   test -s .ai/design.md && echo "design.md found" || echo "ERROR: design.md missing"
   ```
   If missing: stop and tell the user "`.ai/design.md` not found. Run `/design` and get approval first." Do not proceed.

2. Read `.ai/proposal.md` — understand what you're building and why
3. Read `.ai/design.md` — understand the components, interfaces, and tech decisions

You are in the **IMPLEMENT** stage of the pipeline. Write code that matches the design exactly.

## Rules

- **Follow `design.md` exactly.** If you discover the design is incomplete or needs to change, surface the conflict and ask for approval before deviating. Never silently change the architecture.
- **Interview mode.** Write code that is readable and explainable out loud. Avoid clever one-liners. Use clear variable names. Add a comment only when the WHY is non-obvious.
- **No over-engineering.** Build only what `design.md` specifies. Do not add features, abstractions, or patterns not in the design.
- **Commit in logical chunks.** After each meaningful piece of working code, commit. Don't accumulate large uncommitted diffs.

## If the design is unclear

Ask for clarification before writing code. It is better to ask one question now than to build the wrong thing and rewrite it.
