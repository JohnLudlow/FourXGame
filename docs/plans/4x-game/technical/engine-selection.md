# Engine Selection

## Overview

Considerations for choosing a game engine or low-level rendering/input framework. Evaluate requirements for real-time 4x features and platform support.

## Table of contents

- [Overview](#overview)
- [Definition of terms](#definition-of-terms)
- [Feature status](#feature-status)
- [Implementation guide](#implementation-guide)
  - [Requirements](#requirements)
  - [Implementation Steps](#implementation-steps)
- [Phases](#phases)
- [Acceptance criteria](#acceptance-criteria)
- [Testing](#testing)

## Definition of terms

- Engine: runtime/framework providing rendering, input, physics and optionally networking.

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Implementation guide

### Requirements

- Engine must support tile-based worlds, real-time simulation and cross-platform builds.

### Implementation Steps

1. Define technical requirements and evaluate candidate engines (Unity, Godot, custom C++/Mono solution).

2. Prototype core features in shortlisted engines to validate fit.

## Phases

### Phase 1 — Requirements

- Objective: define must-have and nice-to-have engine features.

### Phase 2 — Prototyping

- Objective: implement minimal prototypes to validate candidate engines.

### Phase 3 — Selection

- Objective: choose engine and define integration approach.

## Acceptance criteria

- Chosen engine supports required platforms and performance targets.

## Testing

- Prototype demos and performance validation.
