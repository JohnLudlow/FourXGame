# Characters — Trait Engine

## Overview

TraitEngine is responsible for application, removal, expiration, and persistence of traits and emitting change events.

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
| TraitEngine | Service responsible for trait lifecycle | |

## Feature status

Not started

## Architectural considerations and constraints

- Trait operations must be atomic and emit events to the `EventBus`.
- Must support parameterized traits and duration/stacking rules.

## Implementation guide

### Feature requirements

- (***not-started***) Atomic apply/remove with persisted audit record.
  - GIVEN a trait application request
  - WHEN processed
  - THEN trait is applied and `TraitChanged` event emitted

### Implementation steps

1. Define `ITraitEngine` interface and contract.
2. Implement in-memory + persistent backings with event emission.
3. Add unit tests for stacking, expiration, and concurrency.

## Phases

### Phase 1 — API & semantics

#### Objective

Define behavior (stacking, duration, removal rules).

## Examples

```csharp
public interface ITraitEngine
{
  bool ApplyTrait(Guid characterId, string traitId, IDictionary<string,object>? parameters=null);
  bool RemoveTrait(Guid characterId, string traitId);
}
```
