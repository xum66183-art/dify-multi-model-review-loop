# Architecture

## Overview

The project implements a multi-model quality control loop.

Flow:

```text
Input
 ↓
Generator Model
 ↓
Reviewer Model
 ↓
Structured Score
 ↓
Pass / Fail
```

Pass:

```text
Final Output
```

Fail:

```text
Repair Model
 ↓
Re-review
 ↓
Human Review
```
