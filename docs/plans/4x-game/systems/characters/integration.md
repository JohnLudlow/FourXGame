# Characters — Integration

## Overview

Integration points for `CharacterService` with `CombatSystem`, `CitySystem`, `FactionSystem` and UI.

## Feature status

Not started

## Implementation guide

### Feature requirements

- (***not-started***) Provide `ICharacterReader` and `ICharacterWriter` contracts for consumers.
  - GIVEN a consuming system
  - WHEN it queries character modifiers
  - THEN it receives a consistent modifier set

### Implementation steps

1. Define interfaces and adapter implementations.
2. Add integration tests that mock services and validate modifier propagation.
