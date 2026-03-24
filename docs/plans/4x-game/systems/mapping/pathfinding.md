# Pathfinding and Navigation

## Overview

Pathfinding and navigation for units on a tile-based map. Must support formations, dynamic obstacles, and joining relief forces during battles.

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

- A*: A common graph search algorithm for shortest paths, usable with heuristics on tile grids.
- Formation: positional layout of units within an army that affects movement and combat.

## Feature status

Not started

> Allowed statuses: Not started, In discovery, In design, In development, In test, In review, Completed, Abandoned, Blocked — update this field to the current status for tracking.

## Implementation guide

### Requirements

- Real-time pathfinding that is performant with many agents.
- Support for dynamic re-routing when tiles change (destruction, new obstacles).

### Implementation Steps

1. Implement a grid-aware pathfinding service using A* with movement-cost-aware heuristics.

2. Add hierarchical/path-smoothing layers for large maps and many agents.

3. Integrate with formation system: compute paths for formations and break into unit subpaths.

## Phases

### Phase 1 — Prototype

- Objective: implement basic A* on grid with movement costs.

### Phase 2 — Optimization

- Objective: add hierarchical pathfinding and flow control for many agents.

### Phase 3 — Integration

- Objective: tie into formation systems and dynamic map updates.

## Acceptance criteria

- Pathfinding returns valid, near-optimal paths on varied terrain.
- System supports rerouting when tiles change and maintains performance under load.

## Testing

- Unit tests for path correctness and rerouting on changing maps.
- Stress tests with many agents to measure performance.
