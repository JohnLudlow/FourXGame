# Mod Manifest Specification

## Overview

This document defines the `manifest.json` schema and conventions for mod packages used by the 4x Game. It covers required and optional fields, versioning rules (SemVer), dependency declarations, integrity metadata, load-order hints, and packaging conventions.

## Table of contents

- [Feature status](#feature-status)
- [Definition of Terms](#definition-of-terms)
- [Manifest schema](#manifest-schema)
- [Packaging and layout](#packaging-and-layout)
- [Dependency and versioning rules](#dependency-and-versioning-rules)
- [Integrity, signing and verification](#integrity-signing-and-verification)
- [Load order and conflict resolution hints](#load-order-and-conflict-resolution-hints)
- [Implementation considerations](#implementation-considerations)
- [Testing](#testing)
- [See also](#see-also)
- [References](#references)

## Feature status

- Not started

## Definition of Terms

| Term | Meaning | Reference |
| ---- | ------- | --------- |
| Entry point / `entry` | Optional runtime entry (assembly path, script or resource) that the loader should call or load first. | (Manifest schema) |
| Integrity metadata | Checksum or signature metadata used to verify downloaded manifest and package integrity. | (Security) |
| Manifest | The `manifest.json` file included at the root of a mod package describing id, version, resources and metadata. | (This document) |
| SemVer | Semantic Versioning (MAJOR.MINOR.PATCH) used for mod versions and API compatibility. | <https://semver.org/> |

## Manifest schema

Provide a minimal, versioned JSON schema example and field descriptions. Example (schema version 1):

```json
{
  "schemaVersion": 1,
  "id": "vendor.mod-id",
  "name": "Human readable mod name",
  "version": "1.0.0",
  "publisher": "Example Publisher",
  "description": "Short description",
  "entry": "code/MyMod.dll",
  "dependencies": [
    { "id": "vendor.base", "version": ">=1.0.0 <2.0.0", "required": true }
  ],
  "files": [ "data/", "assets/", "code/" ],
  "loadHints": { "priority": 100 },
  "integrity": { "sha256": "<hex>" },
  "tags": ["gameplay","ui"],
  "compatibility": { "modApiVersion": "1.x" }
}
```

Field notes:

- `schemaVersion` (required): integer schema version.
- `id` (required): reverse-DNS style unique identifier.
- `version` (required): SemVer string.
- `entry` (optional): relative path to code or script to load.
- `dependencies` (optional): array of dependency objects with `id`, `version` range and `required` flag.
- `files` (optional): list of resource paths inside the package.
- `loadHints` (optional): numeric priority or named phases to influence load order.
- `integrity` (optional): checksums for the package or manifest.
- `compatibility` (optional): declared compatible Mod SDK or game API versions.

## Packaging and layout

- Recommended package: ZIP archive named `id-version.zip` containing the manifest at root: `/manifest.json`.
- Resource layout convention: `data/`, `assets/`, `code/`, `meta/`.
- Large assets should be stored under `assets/` and referenced by path in the manifest.

## Dependency and versioning rules

- Use SemVer for `version` and dependency ranges.
- Implement range resolution supporting common operators: `^`, `~`, `>=`, `<` and hyphen ranges.
- Breaking changes in Mod APIs require a major version bump and explicit compatibility statement.
- Missing required dependencies cause load failure for that mod; optional dependencies degrade gracefully.

## Integrity, signing and verification

- Registry fetches must include manifest integrity metadata (sha256) and prefer signed manifests when available.
- The Mod Manager must verify manifest checksums, and optionally verify signatures if the registry provides them.
- Local caching should verify checksums on re-use.

## Load order and conflict resolution hints

- Use `loadHints.priority` (integer) for coarse ordering; lower numbers load earlier.
- When conflicts arise (same asset path in multiple mods), follow the override order: enabled order -> priority -> install time.
- Provide clear diagnostics when conflicts are resolved automatically.

## Implementation considerations

- Schema evolution: keep `schemaVersion` and provide migration guidance for new schema versions.
- Validation: provide a manifest validator tool and unit tests exercising edge cases.
- UX: surface clear error messages for invalid manifests and missing fields.

## Testing

- Unit tests for schema validation, version-range parsing, and integrity verification.
- Integration tests for package extraction, dependency resolution, and conflict resolution behaviors.
- Fuzz tests for malformed manifests and missing fields.

## See also

- `docs/plans/4x-game/modding.md`
- `docs/templates/plan-template.md`

## References

- SemVer: <https://semver.org/>
