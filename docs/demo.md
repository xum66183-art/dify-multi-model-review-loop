# Demo

## Demo Overview

The Basic Review Loop shows how a request can pass through independent generation and quality-control stages before a result is returned.

```text
Input
  ↓
Generator
  ↓
Reviewer
  ↓
Score
  ↓
Decision
  ↓
Repair (optional)
  ↓
Final Output
```

The repair path runs only when the review score is below the configured threshold.

## Example Scenario

A user asks the AI to create a short product introduction.

### Input

```text
Write a concise product introduction for a reusable stainless-steel water bottle.
Mention its 750 ml capacity, leak-resistant lid, and suitability for daily commuting.
Keep the response under 100 words.
```

### Generated Output

```text
Meet a durable stainless-steel water bottle designed for everyday use. Its reusable design helps reduce disposable plastic waste, while the streamlined shape fits easily into a busy routine.
```

### Reviewer

```json
{
  "score": 75,
  "pass": false,
  "issues": [
    "Missing required information about the 750 ml capacity, leak-resistant lid, and daily commuting use."
  ],
  "suggestions": [
    "Add all three required product details while keeping the introduction under 100 words."
  ]
}
```

Because the score is below the default threshold of 80, the workflow routes the result to Repair.

### Repair

```text
Meet the reusable 750 ml stainless-steel water bottle built for daily commuting. Its durable body handles everyday use, while the leak-resistant lid helps keep bags and workspaces dry. The practical capacity offers enough water for trips to work, classes, or errands, helping reduce reliance on disposable plastic bottles.
```

The repaired result is reviewed again before the workflow returns its final output and review metadata.
