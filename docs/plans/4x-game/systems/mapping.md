# Mapping System

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

Procedural map generation and tile management. Map generation uses a tile-based approach; core technique: Wave Function Collapse (WFC). Maps must support in-map battles, dynamic destruction and changes arising from combat.

## Definition of terms

- Wave Function Collapse (WFC): a procedural generation algorithm used to assemble tiled maps by constraint propagation.
- Tile: base unit of the map grid, with terrain, movement cost, and attributes.

## Feature status

Not started

## Implementation guide

### Requirements

- Support tile types, adjacency rules, and deterministic/semi-random generation.
- Generated maps must be queryable for pathfinding and entity placement.

### Implementation Steps

1. Define tile data model (terrain, movement cost, resources).

2. Implement WFC-based generator with adjacency rules and seed control.

3. Provide runtime APIs for querying tiles and applying destruction/modification.

4. Integrate with pathfinding (see pathfinding.md).

## Phases

### Phase 1 — Research & Design

- Objective: define tile taxonomy, adjacency rules and WFC constraints.

### Phase 2 — Implementation

- Objective: implement generator, data models and runtime APIs.

### Phase 3 — Integration

- Objective: connect generator to pathfinding, spawning and map editing.

## Acceptance criteria

- Generated maps conform to adjacency rules and are repeatable with a seed.
- Runtime APIs allow querying and modifying tiles reliably.

## Testing

- Unit tests for adjacency rule enforcement and generator stability across seeds.
- Visual verification of generated maps and deterministic replay.
