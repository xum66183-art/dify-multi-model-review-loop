# Workflows

This directory contains reusable and importable Dify workflow templates.

## Basic Review Loop

`basic-review-loop.yml` is a provider-agnostic quality-control workflow for Dify. It uses multiple model roles to generate, independently review, score, and repair AI output.

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

Node responsibilities:

- **Generator Model** produces the initial result.
- **Reviewer Model** independently evaluates correctness, completeness, consistency, and requirement compliance.
- **Code Node** parses the reviewer's structured JSON score into stable workflow variables.
- **IF/ELSE Node** compares the score with the configured threshold and routes the result either to final output or to the Repair Model.
- **Repair Model** revises failed output using the review feedback before an independent re-review.

## Output Requirements

The Reviewer must return one valid JSON object with no Markdown fences or explanatory text. Its output must satisfy this schema:

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": ["score", "pass", "issues", "suggestions"],
  "properties": {
    "score": {"type": "number", "minimum": 0, "maximum": 100},
    "pass": {"type": "boolean"},
    "issues": {"type": "array", "items": {"type": "string"}},
    "suggestions": {"type": "array", "items": {"type": "string"}}
  }
}
```

The Code Node parses this object. If the Reviewer returns invalid JSON, the workflow uses its fallback review result rather than accepting malformed output.

## Importing into Dify

1. Download `basic-review-loop.yml` from this repository.
2. In Dify, choose **Create App** and import the workflow DSL file.
3. Open the imported workflow and select models available in your workspace.
4. Configure model parameters such as temperature and token limits as appropriate for your selected models.
5. Run a test using `user_input` and adjust the default `threshold` if needed.

The template intentionally contains no API keys, private model configuration, or dependency on a specific model provider. Model availability and exact import options depend on your Dify installation.
