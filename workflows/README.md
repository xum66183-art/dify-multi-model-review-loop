# Workflows

This directory contains reusable and importable Dify workflow templates.

## Basic Review Loop

`basic-review-loop.yml` is a provider-agnostic quality-control workflow for generating, independently reviewing, scoring, and repairing AI output.

The workflow follows this path:

```text
User Input
  ↓
Generator Model
  ↓
Reviewer Model
  ↓
Structured Score
  ↓
Decision
  ├─ Score >= threshold → Final Output
  └─ Score < threshold  → Repair Model → Re-review → Final Repaired Output
```

## Importing into Dify

1. Download `basic-review-loop.yml` from this repository.
2. In Dify, choose **Create App** and import a DSL file.
3. Open the imported workflow.
4. Select models available in your workspace for the Generator, Reviewer, Repair, and Re-review LLM nodes.
5. Keep the reviewer temperature low for more consistent JSON output.
6. Run a test using `user_input` and adjust the default `threshold` if needed.

The template intentionally contains no API keys and does not require a specific model provider. Model availability and exact import options depend on your Dify installation.
