# Controls

## Overview

Controls plan covering input mappings for camera, unit selection, movement, and UI. Support for keyboard/mouse and gamepad where appropriate.

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

- Hotkey: keyboard shortcut for common actions.

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Implementation guide

### Requirements

- Configurable keybindings and input device support.
- Clear selection and command UX for armies and cities.

### Implementation Steps

1. Define default control schemes for keyboard/mouse and gamepad.

2. Implement input mapping layer with rebinding support.

3. Add UI for control customisation and accessibility options.

## Phases

### Phase 1 — Mapping

- Objective: define default mappings and accessibility requirements.

### Phase 2 — Implementation

- Objective: implement input layer with rebinding and device support.

### Phase 3 — UX

- Objective: polish selection and command UX for clarity.

## Acceptance criteria

- Players can rebind keys and select preferred input device.
- Selection and command flows are responsive and discoverable.

## Testing

- Input mapping unit tests and accessibility checks.
