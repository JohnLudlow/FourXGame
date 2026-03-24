# Characters — Events & Bus Integration

## Overview

Defines events emitted for character changes (role assigned, trait changed) and their schema for consumers.

## Feature status

Not started

## Implementation guide

### Feature requirements

- (***not-started***) Emit typed events for role/trait changes consumed by UI and other systems.
  - GIVEN a trait/role change
  - WHEN it completes
  - THEN a typed event is emitted with minimal payload and correlation id

### Implementation steps

1. Define event contracts and versioning policy.
2. Implement publishing in `CharacterService` and `TraitEngine`.
