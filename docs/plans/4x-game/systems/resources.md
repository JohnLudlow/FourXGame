# Resource System

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

Resource management includes physical resources (food, wood, stone, metals, gold, luxuries), magical resources and cultural resources. Resources drive city production, unit recruitment, and faction relations.

## Definition of terms

- Physical resources: tangible items used for production and upkeep.
- Magical resources: rare resources used for spells, units or buildings.

## Feature status

Not started

## Implementation guide

### Requirements

- Resource nodes on map and per-city resource accounting.
- Resources affect happiness, production and diplomacy.

### Implementation Steps

1. Define resource types and their in-game effects.

2. Implement map-based resource nodes and city consumption/production.

3. Add UI and trade systems for resource exchange.

## Phases

### Phase 1 — Model

- Objective: list resource types and their primary game effects.

### Phase 2 — Systems

- Objective: implement resource nodes, city accounting and basic trade.

### Phase 3 — Economy

- Objective: integrate with diplomacy, market mechanics and scarcity tuning.

## Acceptance criteria

- Resource nodes appear on maps and produce expected yields.
- City accounting tracks production, consumption and stockpiles correctly.

## Testing

- Unit tests for resource accounting and trade operations.
