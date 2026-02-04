# Saving, Autosaves, and Game Management

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

## Overview

Persisting game state with support for manual saves, autosaves, and quicksaves. Save format should be versioned and robust against partial writes.

## Definition of terms

- Autosave: periodic automatic save of game state.
- Save snapshot: serialized representation of full game state.

## Feature status

Not started

## Implementation guide

### Requirements

- Versioned save format and migration support.
- Safe write (temp file then rename) to avoid corruption.

### Implementation Steps

1. Design save schema and versioning strategy.

2. Implement serialization and safe write APIs.

3. Add autosave triggers and UI management for load/save.

## Phases

### Phase 1 — Format

- Objective: design versioned save schema and migration paths.

### Phase 2 — Implementation

- Objective: implement serialization, safe write and load APIs.

### Phase 3 — UX & Recovery

- Objective: implement autosave UX, recovery on corrupt save and migration tooling.

## Acceptance criteria

- Saves and loads restore game state reliably across versions (with migration).
- Autosave and manual saves do not corrupt existing saves on partial write.

## Testing

- Tests for save/load roundtrip and migration between versions.
