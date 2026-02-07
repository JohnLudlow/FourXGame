# Platform Support

## Overview

Platform support considerations (PC first, then consoles and mobile) including input, performance, and packaging differences.

## Table of contents

- [Platform Support](#platform-support)
  - [Overview](#overview)
  - [Table of contents](#table-of-contents)
  - [Definition of terms](#definition-of-terms)
  - [Feature status](#feature-status)
  - [Implementation guide](#implementation-guide)
    - [Requirements](#requirements)
    - [Implementation Steps](#implementation-steps)
  - [Phases](#phases)
    - [Phase 1 — Scoping](#phase-1--scoping)
    - [Phase 2 — Abstraction](#phase-2--abstraction)
    - [Phase 3 — Validation](#phase-3--validation)
  - [Acceptance criteria](#acceptance-criteria)
  - [Testing](#testing)

## Definition of terms

- Platform: target OS/hardware such as Windows, Linux, macOS, consoles, mobile.

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Implementation guide

### Requirements

- List supported platforms and platform-specific constraints.

### Implementation Steps

1. Define minimum supported platforms and their constraints.

2. Implement platform abstraction for filesystem, input and audio.

## Phases

### Phase 1 — Scoping

- Objective: decide initial supported platforms and constraints.

### Phase 2 — Abstraction

- Objective: implement platform abstractions for OS-specific APIs.

### Phase 3 — Validation

- Objective: run cross-platform packaging and smoke tests.

## Acceptance criteria

- Core game runs on declared supported platforms with acceptable performance.

## Testing

- Cross-platform smoke tests and packaging verification.
