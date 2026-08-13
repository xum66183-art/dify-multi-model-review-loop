# Workflow Overview

This document explains the main nodes in `workflows/basic-review-loop.yml`. No screenshot is included because the project does not currently have a verified image captured from an imported Dify workspace.

```text
User Input
  ↓
Generator
  ↓
Reviewer
  ↓
Code (parse score)
  ↓
IF/ELSE
  ├─ Pass → Final Output
  └─ Fail → Repair → Re-review → Final Repaired Output
```

## Generator Node

The Generator LLM receives the user's request and produces the initial candidate result. The template does not require a specific model provider.

## Reviewer Node

The Reviewer LLM evaluates the candidate independently for correctness, completeness, consistency, and requirement compliance. It must return the documented JSON structure.

## Code Node

The Code node parses the Reviewer JSON, normalizes the score, and compares it with the configured threshold. Invalid JSON produces a safe failing review rather than being accepted.

## IF/ELSE Node

The IF/ELSE node reads the Boolean result produced by the Code node:

- A passing score routes to Final Output.
- A failing score routes to Repair.

## Repair Node

The Repair LLM receives the original request, initial result, and review feedback. It revises the candidate and sends the repaired result to an independent re-review before final output.
