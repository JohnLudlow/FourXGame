# Factions System

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

Factions are led by characters and defined by their leaders' agendas. Multiple faction types exist: national, racial, religious, political, economic and cultural.

## Definition of terms

- National faction: player-controlled or peer political entity.
- Racial faction: linked to a race and reacts to race-targeted actions.
- Religious faction: linked to beliefs and reacts to religious events.

## Feature status

Not started

## Implementation guide

### Requirements

- Represent faction types and their relationships to characters and territories.
- Factions react to player actions (e.g., pogroms, conversions).

### Implementation Steps

1. Implement faction data model and relation to characters and territories.

2. Define reaction rules for faction types to world events.

3. Provide diplomacy and faction-internal politics APIs.

## Phases

### Phase 1 — Model

- Objective: specify faction types, relations and event hooks.

### Phase 2 — Implementation

- Objective: implement faction engine, reaction rules and APIs.

### Phase 3 — Diplomacy

- Objective: build diplomacy UI and negotiation flows.

## Acceptance criteria

- Faction relationships and reactions to events are recorded and surface in diplomacy choices.

## Testing

- Scenario tests for faction reactions to representative events.
