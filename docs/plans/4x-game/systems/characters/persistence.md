# Characters — Persistence

## Overview

Persistence layer for `Character` objects and trait/change audit records.

## Feature status

Not started

## Implementation guide

### Feature requirements

- (***not-started***) Persist characters and trait-change audit logs with versioning.
  - GIVEN a character update
  - WHEN saved
  - THEN it is durable and auditable

### Implementation steps

1. Define DB schema and indices for character lookups.
2. Implement optimistic concurrency and save hooks.
