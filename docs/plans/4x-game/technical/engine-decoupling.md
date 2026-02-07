# Engine Decoupling

## Overview

Design guidelines to decouple game logic from engine-specific APIs to allow portability and easier testing. Keep simulation deterministic and engine-agnostic where possible.

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

- Decoupling: separating game logic from engine/platform-specific code.

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Implementation guide

### Requirements

- Clear separation between simulation models and rendering/input layers.

### Implementation Steps

1. Define engine-agnostic interfaces for simulation, audio and input.

2. Implement adapters for the chosen engine.

## Phases

### Phase 1 — Interface design

- Objective: define stable interfaces for core systems.

### Phase 2 — Adapter implementation

- Objective: create adapters for engine-specific bindings.

### Phase 3 — Testing

- Objective: verify simulation correctness under headless runners.

## Acceptance criteria

- Game logic can be executed without the engine present (headless mode).

## Testing

- Unit tests for simulation logic using headless runners.
