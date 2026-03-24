# Characters — Role System

## Overview

Defines role types, assignment rules, and role-effect calculation plumbing for systems (combat, city bonuses, councils).

## Table of contents

- [Overview](#overview)
- [Definition of terms](#definition-of-terms)
- [Feature status](#feature-status)
- [Architectural considerations and constraints](#architectural-considerations-and-constraints)
- [Implementation guide](#implementation-guide)
- [Phases](#phases)
- [Examples](#examples)

## Definition of terms

| Term | Meaning | Reference |
| ---- | ------- | --------- |
| Role | Named assignment that grants effects | |

## Feature status

Not started

## Architectural considerations and constraints

- Roles should be orthogonal to traits and represented as ID+parameters for extensibility.

## Implementation guide

### Feature requirements

- (***not-started***) Enforce single role per role-type and persist assignment.
  - GIVEN a role assignment
  - WHEN applied
  - THEN the assignment is stored and role effects are available to calculators

### Implementation steps

1. Define role types and storage.
2. Implement assignment API with validation.
3. Wire calculators in consuming systems.

## Examples

```csharp
public enum RoleType { General, Governor, CouncilMember }
public record RoleAssignment(RoleType Type, string RoleId, Guid? AssignedAt = null);
```

## Phases

### Phase 1 — Role semantics

#### Objective

Define role types, constraints, and validation rules.
