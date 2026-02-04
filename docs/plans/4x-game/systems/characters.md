# Characters System

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

Characters are central to the game: they can lead armies, govern cities, join councils, and gain traits from actions. Characters provide bonuses and have agendas that influence faction behaviour.

## Definition of terms

- Trait: a persistent modifier gained by characters through actions.
- Agenda: a character-driven set of priorities that influences faction choices.

## Feature status

Not started

## Implementation guide

### Requirements

- Characters must be able to occupy roles (general, governor, council member).
- Characters gain and lose traits via actions and events.

### Implementation Steps

1. Design character data model (stats, roles, traits, agendas).

2. Implement trait system with triggers based on actions.

3. Provide UI for character management and council interactions.

## Phases

### Phase 1 — Data modeling

- Objective: establish canonical character schema, roles and trait model.

### Phase 2 — Systems

- Objective: implement trait triggers, role effects and UI surfaces.

### Phase 3 — Integration

- Objective: connect characters to factions, armies and city systems.

## Acceptance criteria

- Characters can be assigned roles and their effects apply correctly.
- Traits are persistently stored and influence relevant calculations.

## Testing

- Unit tests for trait application and role effects.
- Scenario tests for character-driven faction decisions.
