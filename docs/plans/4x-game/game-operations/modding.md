# Game Operations — Modding, Mod Development & Mod Management

## Overview

This child feature describes the Game Operations capability for modding: the end-to-end support for creating, packaging, discovering, installing, running, and managing mods for the 4x Game. It covers the developer-facing Mod SDK and tooling, the player-facing in-game Mod Manager, the decentralised registry model, and runtime safety and performance constraints.

## Table of contents

- [Feature status](#feature-status)
- [Definition of Terms](#definition-of-terms)
- [Requirements](#requirements)
- [Implementation Steps](#implementation-steps)
- [Implementation Considerations](#implementation-considerations)
- [Testing](#testing)
- [Deliverables](#deliverables)
- [See also](#see-also)
- [References](#references)

## Feature status

- In design

## Definition of Terms

| Term | Meaning | Reference |
| ---- | ------- | --------- |
| Base mod | The bundled core-content mod shipped with the game that provides default assets and rules. | (Project convention) |
| Dependency resolution | Algorithm to compute mod load order and handle required/optional dependencies and version constraints. | (Project convention) |
| Manifest | A JSON file included in a mod package describing `id`, `name`, `version`, `publisher`, `dependencies`, `files`, and load-order hints. | (Phase 2: manifest spec) |
| Mod | A packaged change to game content or behaviour. Can be data-only (JSON/YAML/assets) or a compiled .NET assembly that uses the Mod SDK. | (Project convention) |
| Mod SDK | The developer-facing API and libraries that enable mods to integrate with the game engine safely and consistently. | (Project convention) |
| Registry / Bucket | A decentralised list of mod manifests (a repository URL or index) that the Mod Manager can query to discover mods. | (Project convention) |
| Sandbox | Runtime isolation strategy (process, AppDomain, assembly load context with restricted permissions, or other) to limit unsafe mod behaviour. | (Project convention) |
| SemVer | Semantic Versioning rules (MAJOR.MINOR.PATCH) used for mod versions and mod-API compatibility. | <https://semver.org/> |

## Requirements

### Functional

- F1: Mod package format: support a ZIP-based bundle with a required `manifest.json` and a declared resource layout.
- F2: Manifest schema: define required fields (`id`, `name`, `version`, `entry`, `dependencies[]`) and optional fields (load hints, tags, compatibility matrix).
- F3: Mod types: support data-only mods, asset-only mods, and code mods (compiled against the Mod SDK).
- F4: Mod Manager (in-game): discover registries, list mods, show metadata, install, update, enable/disable, uninstall, and show conflict/dependency warnings.
- F5: Registry support: allow adding/removing registries (buckets) by URL, fetch manifests with integrity verification (signed manifests or checksum).
- F6: Dependency resolution: implement SemVer-aware resolution and clear conflict resolution UI for users.
- F7: Safety: provide a default sandbox for code mods; require explicit user opt-in for less-restricted execution modes.
- F8: Developer experience: publish a Mod SDK, sample mods, and a CLI/tooling to pack/unpack and validate manifests.
- F9: Diagnostics: log mod loading events, errors, and performance counters; include mod metadata in diagnostics bundles.
- F10: Base mod: represent base game content as an updatable base mod and document override rules.

### Non-functional

- N1: Performance: mod loading on startup must not increase cold start by more than X ms (define X during implementation). Lazy-load large assets where possible.
- N2: Reliability: loading a faulty mod must not crash the game; failures should be isolated and reported.
- N3: Security: integrity checks for registry manifests; sandboxing and explicit consent flows for code mods.
- N4: Usability: Mod Manager flows must be understandable to non-technical players; error messages must be actionable.

## Implementation Steps

1. Design phase (spec):
   - Draft `manifest.json` schema and sample manifests for data-only and code mods.
   - Define mod package layout and recommended conventions.
   - Choose sandbox model options and document trade-offs.
2. Developer tooling:
   - Create `mod-cli` (pack, unpack, validate-manifest, sign) and publish sample mod templates.
   - Implement initial Mod SDK surface (versioned assembly, API surface for registration, events, assets hooks).
3. Runtime and loader:
   - Implement manifest parser, package extractor, and resource resolver.
   - Implement dependency resolver using SemVer and load-order algorithm.
   - Implement sandboxed loader for code mods; fallback to disabled mode with clear user consent.
4. Mod Manager UI:
   - Wire discovery UI to fetch manifests from registries.
   - Implement install/update/uninstall flows and an enable/disable toggle that persists per-save metadata.
   - Add conflict resolution and dependency visualization UI.
5. Registry design:
   - Define a minimal registry API (static index file or simple HTTP manifest index) and integrity policies (checksums, optional signatures).
   - Provide a reference registry bucket and sample bucket spec.
6. Testing and diagnostics:
   - Add unit tests for manifest parsing, dependency resolution, and manifest validation.
   - Add integration tests for install/uninstall, load/unload, and safe error handling.
7. Documentation and samples:
   - Write Mod SDK docs, manifest specification, packaging guide, and a tutorial sample mod.
8. Beta & rollout:
   - Ship as opt-in in a pre-release stream; collect diagnostics bundles (local) to iterate on stability and UX.

## Implementation Considerations

Readability & Maintainability

- Keep the manifest schema minimal and extensible; prefer clear field names and version the schema.

Reliability & Testability

- Instrument mod loading to allow replay of failures using captured manifests and sample packages.
- Provide a "safe mode" startup flag that disables code mods to assist in recovery.

Security & Privacy

- Default to conservative permissions for code mods. Require explicit opt-in and a clear consent screen for any elevated capabilities.
- Do not collect or transmit player-identifying data in diagnostics bundles by default.

Performance

- Defer heavy asset loading until needed; cache extracted asset metadata to speed subsequent runs.
- Measure and set concrete thresholds for acceptable load-time overhead.

Compatibility & Upgrade Path

- Define compatibility policy: major Mod SDK changes require major bump and migration guide.
- Support disabling incompatible mods automatically with clear user notification.

Registry Trust Model

- Allow multiple registries; make integrity verification pluggable (signed manifests, checksums).
- Provide UI indicators for registry trust (e.g., official vs community buckets).

UX

- Display clear, non-technical explanations for mod conflicts and required actions.
- Validate mod actions (install/remove) with helpful undo/rollback where feasible.

## Testing

Unit tests

- Manifest schema validation tests for valid and invalid manifests.
- Dependency resolution tests (version ranges, missing deps, cyclic deps).

Integration tests

- Install/uninstall flow tests including registry fetching and integrity checks.
- Load-time isolation tests for faulty mods (ensure game continues running).

Performance tests

- Startup time metrics with varying numbers and sizes of mods.
- Asset streaming and memory usage tests.

Manual / Acceptance tests

- Mod Manager usability walkthrough (discover, install, enable, disable, update, remove).
- Create diagnostics bundle after mod failure and verify bundle contents.
- Create and run a sample mod that modifies a game rule and verify expected behaviour.

Security tests

- Sandbox escape and permission boundary tests for code mods.
- Registry integrity tampering tests (modified manifest checksums) and expected failure handling.

## Deliverables

- `docs/plans/4x-game/modding.md` (this document)
- `docs/plans/4x-game/game-operations/modding/manifest-spec.md` (detailed manifest schema — child plan)
- Sample Mod SDK nuget/package and CLI tooling (implementation)
- Reference registry bucket and sample mods

(If you'd like, I can now create the follow-up `manifest-spec.md` under `docs/features/modding/` and scaffold example manifests and a sample-mod template.)

## See also

- `docs/plans/4x-game.md` (parent system-level plans)
- `docs/templates/plan-template.md` (plan template)

## References

- SemVer: <https://semver.org/>
