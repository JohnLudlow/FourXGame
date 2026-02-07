# Combat and Battle System

## Overview

Combat occurs on the main map (no separate battle map). Armies use formations; battles can change map state. Relief forces can join, and destruction affects terrain and tiles.

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

- Formation: organization of units (infantry centre, cavalry flanks).
- Relief force: reinforcements that join an ongoing battle.

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Implementation guide

### Requirements

- Real-time combat on the map with formation support.
- Battle effects must modify the map (destruction, blocked tiles).

### Implementation Steps

1. Design combat rules and unit interactions at formation level.

2. Implement formation movement and engagement logic integrated with pathfinding.

3. Add map-modification hooks for damage and persistent effects.

## Phases

### Phase 1 — Design

- Objective: specify formation model and combat resolution rules.

### Phase 2 — Implementation

- Objective: implement formation movement, engagement and reinforcement mechanics.

### Phase 3 — Persistence

- Objective: ensure battle effects persist on the map and influence subsequent gameplay.

## Acceptance criteria

- Battles resolve using formation rules and can accept reinforcements mid-battle.
- Damage from battles updates tile state and blocks/unblocks movement appropriately.

## Testing

- Simulation tests for formation behavior and reinforcement joining.
