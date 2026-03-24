# Characters — Performance

## Overview

Performance goals and benchmarks for character-related hot paths (trait lookup, modifier calculation).

## Feature status

Not started

## Implementation guide

### Feature requirements

- (***not-started***) Trait lookup and modifier calculation must add <2ms per action on mid-tier hardware.

### Implementation steps

1. Create microbenchmarks for trait lookup and modifier application.
2. Optimize data structures and caching.
