---
name: linear-cli
description: Manage personal Linear issues via the linear CLI. Use when the user mentions Linear, issues, or linear commands.
---

# Linear CLI

## Discovery

Flags and subcommands: always `linear <cmd> --help`. Do not guess options.

## Rules

- Your issues → `issue mine` (alias: `issue list`)
- Search / filter beyond "mine" → `issue query`
- Multi-line markdown → `--description-file` / `--body-file` (never inline with `\n`)
- Scripts / non-interactive → `--no-interactive`
- Structured output → `--json`

## Recipes

```bash
linear issue mine --state started --sort priority
linear issue query --state backlog --json
linear issue create --title "Fix login redirect" \
  --description-file ./description.md --no-interactive
linear issue update ENG-123 --state "In Review"
linear issue update ENG-123 --add-label bug
linear issue comment add ENG-123 --body-file ./comment.md
linear issue view ENG-123
linear issue view ENG-123 --json
linear issue url ENG-123
```
