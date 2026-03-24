# Diagnostics

## Overview

Runtime diagnostics for logging, error collection and developer tools to inspect simulation state during development and in the field.

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

- Diagnostics: tools and telemetry for understanding runtime behaviour.

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Implementation guide

### Requirements

- Configurable logging levels and collection of relevant traces.

### Implementation Steps

1. Implement structured logging and error reporting hooks.

2. Add in-engine developer console and snapshot capture tools.

## Phases

### Phase 1 — Logging

- Objective: implement structured logging and basic error reporting.

### Phase 2 — Tools

- Objective: add developer console and snapshot capture.

### Phase 3 — Field Telemetry

- Objective: integrate error aggregation and remote diagnostics.

## Acceptance criteria

- Logs and diagnostics provide sufficient context to reproduce and fix issues.

## Testing

- Verify logs and error reports produce actionable information for crashes and misbehaviour.
