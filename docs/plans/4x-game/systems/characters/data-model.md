# Characters — Data Model

## Overview

Defines the canonical `Character` schema, field semantics, serialization, and migration considerations.

## Table of contents

- [Overview](#overview)
- [Definition of terms](#definition-of-terms)
- [Feature status](#feature-status)
- [Architectural considerations and constraints](#architectural-considerations-and-constraints)
- [Implementation guide](#implementation-guide)
- [Phases](#phases)
- [Examples](#examples)
- [See also](#see-also)

## Definition of terms

| Term | Meaning | Reference |
| ---- | ------- | --------- |
| CharacterId | Stable GUID identity for characters | |
| Stat | Named numeric attribute (e.g., Strength) | |

## Feature status

Not started

## Architectural considerations and constraints

- Schema must be compact for network sync and stable for persistence.
- Use versioned serialization to support rolling updates.

## Implementation guide

### Feature requirements

- (***not-started***) Character schema persisted and round-trip serializable.
  - GIVEN a Character instance
  - WHEN serialized and deserialized
  - THEN it equals the original instance

### Implementation steps

1. Define `Character` record with fields and docs.
2. Add serialization tests and version marker.
3. Add migration strategy for older saved formats.

## Phases

### Phase 1 — Schema design

#### Objective

Design fields, types, and constraints.

#### Technical details

- Use IDs for traits/roles and parameter blobs for extensibility.

## Examples

```csharp
// docs/examples/CharacterModel.cs
public record Character(Guid Id, string Name)
{
  public Dictionary<string,int> Stats { get; init; } = new();
  public HashSet<string> Traits { get; init; } = new();
}
```

## See also

- ../trait-engine.md
