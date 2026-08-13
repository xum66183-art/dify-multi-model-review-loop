# Dify Multi-Model Review Loop

A reusable quality-control workflow for Dify that combines multi-model generation, independent review, structured scoring, automatic repair, and human-in-the-loop fallback.

## Overview

Large language model outputs are often inconsistent when used in production workflows. A single model may generate content that looks correct but contains logical errors, missing requirements, formatting problems, or unsupported information.

**Dify Multi-Model Review Loop** provides a reusable workflow architecture for improving output reliability by separating generation from evaluation.

The basic workflow is:

```text
Input
  ↓
Generator
  ↓
Independent Reviewer
  ↓
Structured Score
  ↓
Pass / Fail
 ↙       ↘
Pass     Repair
           ↓
       Re-review
           ↓
     Human Review
```

## Requirements

- A Dify environment with Workflow applications and DSL import support
- At least one model available in your Dify workspace
- Permission to configure the Generator, Reviewer, Repair, and Re-review model nodes

The project is provider-agnostic: you select and configure model providers available in your own workspace. The repository does not include API keys or provider credentials.

## Quick Start

1. Prepare a Dify environment that supports Workflow DSL imports.
2. Download [`workflows/basic-review-loop.yml`](workflows/basic-review-loop.yml).
3. In Dify, create an application by importing the Workflow DSL file.
4. Open the imported workflow and select models available in your workspace for the Generator, Reviewer, Repair, and Re-review nodes.
5. Configure model parameters appropriate for your selected models.
6. Set the `threshold` input. The default is `80`; results with `score >= threshold` pass, while lower scores enter Repair.
7. Run the workflow with [`examples/input-example.md`](examples/input-example.md), then compare the review structure with [`examples/output-example.md`](examples/output-example.md).

For node details and additional configuration guidance, see the [Workflow documentation](workflows/README.md).

## Project Architecture

See [`docs/architecture.md`](docs/architecture.md) for the project architecture and review-loop design. A node-by-node explanation is available in [`docs/workflow-overview.md`](docs/workflow-overview.md).

## Demo

See [`docs/demo.md`](docs/demo.md) for a complete example that follows a request through generation, review, scoring, optional repair, and final output.

## Features

- Multi-model generation and review
- Independent reviewer model
- Structured quality scoring
- Configurable pass threshold
- Automatic repair loop
- Human-in-the-loop fallback
- Provider-agnostic workflow design
- Reusable Dify workflow templates
- Example prompts and test inputs

## Why This Project Exists

AI applications often rely on a single model to both generate and implicitly validate its own output.

This creates several problems:

- hallucinations may pass unnoticed
- required constraints may be ignored
- structured output may fail
- logical inconsistencies may remain
- repeated generation can produce unstable quality

This project introduces an independent review stage and explicit quality-control loop before the final output is accepted.

## Example Review Output

```json
{
  "score": 86,
  "pass": true,
  "issues": [],
  "suggestions": []
}
```

When the score is below the configured threshold, the workflow sends the result to a repair model and performs another review.

If repeated repair attempts still fail, the workflow can route the task to human review.

## Planned Workflow Templates

The repository will include reusable templates for:

1. Basic generation + review
2. Multi-model independent review
3. Automatic repair loop
4. Human-review fallback
5. Structured JSON evaluation
6. Multi-provider model configuration

## Supported Model Providers

The workflow architecture is designed to work with different model providers supported by Dify, including:

- OpenAI
- Anthropic Claude
- Google Gemini
- DeepSeek
- OpenAI-compatible endpoints

Specific model availability depends on your own Dify environment and provider configuration.

## Repository Structure

```text
dify-multi-model-review-loop/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── CONTRIBUTING.md
├── workflows/
├── prompts/
├── examples/
├── docs/
└── screenshots/
```

## Project Status

🚧 **Early Development**

The first reusable workflow templates and documentation are currently being prepared.

The initial public release will focus on the core:

**Generate → Review → Score → Repair → Human Review**

pipeline.

## Roadmap

### v0.1.0

- [ ] Basic Dify review-loop workflow
- [ ] Generator prompt template
- [ ] Reviewer prompt template
- [ ] Structured scoring output
- [ ] Automatic repair workflow
- [ ] Example input/output
- [ ] Installation documentation

### v0.2.0

- [ ] Multi-provider configuration examples
- [ ] Improved structured-output handling
- [ ] Reviewer failure fallback
- [ ] Human-review fallback
- [ ] Additional workflow examples

## Contributing

Contributions, bug reports, workflow improvements, and documentation suggestions are welcome.

Please open an Issue if you encounter a problem or have an idea for improving the workflow.

## License

This project is licensed under the MIT License.
