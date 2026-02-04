# Onboarding and Tutorial System

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

A progressive onboarding and tutorial system introducing core gameplay concepts: exploration, city management, armies and battles, characters and factions, and resources. Tutorials are contextual and triggered by player actions or milestones.

## Definition of terms

- Tutorial: a guided, interactive lesson introducing a feature.
- Onboarding: a sequence of introductory tutorials, tips and UI highlights for new players.

## Feature status

Not started

## Implementation guide

### Requirements

- Players must be able to opt-in/out of tutorials.
- Tutorials must be resumable and skippable.

### Implementation Steps

1. Design tutorial scripts for core systems: mapping, characters, factions, combat, resources.

2. Implement a tutorial manager capable of listening to game events and triggering steps.

3. Create UI overlay components for hints and step progress.

4. Add analytics hooks to measure tutorial completion.

## Phases

### Phase 1 — Discovery

- Objective: define core tutorial topics and player flows.

### Phase 2 — Implementation

- Objective: implement tutorial manager, UI overlays and basic scripts.

### Phase 3 — Polish

- Objective: refine pacing, add analytics and localization.

## Acceptance criteria

- Tutorials can be enabled/disabled in settings.
- A tutorial can be paused and resumed without losing progress.
- Tutorial manager fires and completes steps based on game events.

## Testing

- Unit tests for tutorial manager state transitions.
- Playtests for tutorial clarity and pacing.
