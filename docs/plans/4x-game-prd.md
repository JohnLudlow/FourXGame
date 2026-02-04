<!-- vscode-markdown-toc -->
* 1. [Overview](#Overview)
* 2. [Table of contents](#Tableofcontents)
* 3. [Feature status](#Featurestatus)
* 4. [Definition of Terms](#DefinitionofTerms)
* 5. [Architectural considerations and constraints](#Architecturalconsiderationsandconstraints)
	* 5.1. [Product constraints](#Productconstraints)
	* 5.2. [Modding constraints](#Moddingconstraints)
	* 5.3. [Diagnostics constraints](#Diagnosticsconstraints)
	* 5.4. [Open architectural questions (placeholders)](#Openarchitecturalquestionsplaceholders)
* 6. [Implementation guide](#Implementationguide)
	* 6.1. [Feature requirements](#Featurerequirements)
	* 6.2. [Phase 1 - Executive summary and product scope](#Phase1-Executivesummaryandproductscope)
		* 6.2.1. [Objective](#Objective)
		* 6.2.2. [Technical details](#Technicaldetails)
		* 6.2.3. [Phase requirements](#Phaserequirements)
	* 6.3. [Phase 2 - Modding and content pipeline](#Phase2-Moddingandcontentpipeline)
		* 6.3.1. [Objective](#Objective-1)
		* 6.3.2. [Technical details](#Technicaldetails-1)
		* 6.3.3. [Phase requirements](#Phaserequirements-1)
	* 6.4. [Phase 3 - Diagnostics and opt-in telemetry](#Phase3-Diagnosticsandopt-intelemetry)
		* 6.4.1. [Objective](#Objective-1)
		* 6.4.2. [Technical details](#Technicaldetails-1)
		* 6.4.3. [Phase requirements](#Phaserequirements-1)
	* 6.5. [Phase 4 - MVP vertical slice definition](#Phase4-MVPverticalslicedefinition)
		* 6.5.1. [Objective](#Objective-1)
		* 6.5.2. [Technical details](#Technicaldetails-1)
		* 6.5.3. [Phase requirements](#Phaserequirements-1)
	* 6.6. [Phase 5 - Risks and roadmap](#Phase5-Risksandroadmap)
		* 6.6.1. [Objective](#Objective-1)
		* 6.6.2. [Technical details](#Technicaldetails-1)
	* 6.7. [Phase 6 - PRD review and sign-off](#Phase6-PRDreviewandsign-off)
		* 6.7.1. [Objective](#Objective-1)
		* 6.7.2. [Technical details](#Technicaldetails-1)
* 7. [See also](#Seealso)
* 8. [References](#References)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc --># 4x Game - Product Requirements Document (Scaffold)

##  1. <a name='Overview'></a>Overview

This document scaffolds the missing information needed to represent a complete Product Requirements Document (PRD) for the 4x Game.

It complements (and does not replace) the existing system plans under `docs/plans/4x-game.md` and `docs/plans/4x-game/`.

High-level constraints confirmed so far:

- MVP platform support is Windows only.
- For a full release, Linux support should be considered.
- macOS support is not required.
- Single-player only; no multiplayer support planned.
- Cloud saves are not required.
- Remote telemetry is not required for the MVP.
  - Comprehensive local logging and diagnostics are required (local log or dump files).
  - Remote telemetry may be required later, but must be opt-in.
- Modding is required and is intended to become a core part of the experience.
  - Modding should be opt-in and decentralised.
  - For the MVP, base game content can be implemented as a bundled base mod (similar to Factorio’s base mod).
  - Consider an in-game mod manager and a lightweight, public mod registry (similar in spirit to Scoop “buckets”).

##  2. <a name='Tableofcontents'></a>Table of contents

- [Feature status](#feature-status)
- [Definition of Terms](#definition-of-terms)
- [Architectural considerations and constraints](#architectural-considerations-and-constraints)
- [Implementation guide](#implementation-guide)
  - [Feature requirements](#feature-requirements)
  - [Phase 1 - Executive summary and product scope](#phase-1---executive-summary-and-product-scope)
  - [Phase 2 - Modding and content pipeline](#phase-2---modding-and-content-pipeline)
  - [Phase 3 - Diagnostics and opt-in telemetry](#phase-3---diagnostics-and-opt-in-telemetry)
  - [Phase 4 - MVP vertical slice definition](#phase-4---mvp-vertical-slice-definition)
  - [Phase 5 - Risks and roadmap](#phase-5---risks-and-roadmap)
  - [Phase 6 - PRD review and sign-off](#phase-6---prd-review-and-sign-off)
- [See also](#see-also)
- [References](#references)

##  3. <a name='Featurestatus'></a>Feature status

In discovery

##  4. <a name='DefinitionofTerms'></a>Definition of Terms

| Term | Meaning | Reference |
| ---- | ------- | --------- |
| Base mod | A mod shipped with the game that provides the default content (units, buildings, rules, and so on). It can be treated like any other mod and can be overridden or extended by other mods. | (Project convention; inspired by Factorio’s base mod concept) |
| Bucket | A decentralised list of mod “manifests” (metadata) hosted in a public repository, used by the mod manager to discover mods. | (Analogy to Scoop buckets; exact design is TBD) |
| Decentralised mod registry | A mod discovery mechanism that does not rely on a single central service; users can opt into one or more registries (“buckets”). | (Project convention; design TBD) |
| Mod | A package that changes or adds game content or behaviour. The supported scope (data only, scripts, assets, and so on) must be explicitly defined in the PRD. | (Project convention) |
| Mod manager | In-game tooling to install, update, enable, and disable mods, and to resolve mod dependencies and conflicts. | (Project convention; inspired by Factorio’s mod manager) |
| MVP | Minimum viable product: the smallest coherent version that can be shipped to players and provide a complete “core loop”. | <https://en.wikipedia.org/wiki/Minimum_viable_product> |
| Opt-in | A user choice that is disabled by default and must be explicitly enabled by the player. | <https://en.wikipedia.org/wiki/Opt-in_email> |
| Telemetry | Automated collection of usage and error data for analysis. In this project, remote telemetry is out of scope for MVP and may be added later as opt-in only. | <https://en.wikipedia.org/wiki/Telemetry> |
| Vertical slice | A thin but complete end-to-end piece of the product that crosses multiple systems (for example: map generation to combat to save/load). | (Common product development term) |

##  5. <a name='Architecturalconsiderationsandconstraints'></a>Architectural considerations and constraints

###  5.1. <a name='Productconstraints'></a>Product constraints

- The MVP MUST run on Windows.
- The MVP MUST be single-player.
- The MVP MUST NOT require a network connection to play.
- The MVP MUST NOT require cloud services for saving.

###  5.2. <a name='Moddingconstraints'></a>Modding constraints

- Modding MUST be opt-in.
- Mod discovery and installation SHOULD support decentralised registries (“buckets”).
- The base game content SHOULD be expressible as a bundled base mod.

###  5.3. <a name='Diagnosticsconstraints'></a>Diagnostics constraints

- The MVP MUST provide comprehensive diagnostics via local log files and/or dump files.
- If remote telemetry is implemented later, it MUST be opt-in and MUST clearly describe what data is collected.

###  5.4. <a name='Openarchitecturalquestionsplaceholders'></a>Open architectural questions (placeholders)

- What is the intended approach for deterministic simulation (if required)? (TBD)
- What data formats are intended for content and mods (for example JSON, YAML, custom binary)? (TBD)
- Will mods be data-only for MVP, or is scripting part of MVP? (TBD)

##  6. <a name='Implementationguide'></a>Implementation guide

This is a documentation implementation plan (not a code implementation plan): it describes what must be documented to complete the PRD.

###  6.1. <a name='Featurerequirements'></a>Feature requirements

- (***Not started***) The PRD MUST define the core problem statement and the product pillars.
  - GIVEN the existing system plans
  - WHEN the PRD is written
  - THEN it includes a clear problem statement and 3 to 5 product pillars that differentiate the game

- (***Not started***) The PRD MUST define measurable success criteria for the MVP.
  - GIVEN an MVP scope
  - WHEN success criteria are stated
  - THEN each criterion is measurable (for example: stability, performance, onboarding completion)

- (***Not started***) The PRD MUST explicitly define platform scope and non-goals.
  - GIVEN platform requirements are known
  - WHEN the PRD scope is read
  - THEN it explicitly states Windows-only for MVP, Linux considered later, and macOS out of scope

- (***Not started***) The PRD MUST define modding scope and the mod manager experience.
  - GIVEN modding is a core requirement
  - WHEN the PRD describes modding
  - THEN it defines mod capabilities, packaging, dependencies, conflict rules, and mod manager user flows

- (***Not started***) The PRD MUST define diagnostics requirements for MVP and opt-in telemetry requirements for post-MVP.
  - GIVEN MVP has no remote telemetry
  - WHEN diagnostics are specified
  - THEN local logs and dumps are defined, and remote telemetry (if any) is clearly opt-in

###  6.2. <a name='Phase1-Executivesummaryandproductscope'></a>Phase 1 - Executive summary and product scope

***Not started***

####  6.2.1. <a name='Objective'></a>Objective

Create the missing PRD-level "why" and "what" content that is not currently present in the system plans.

####  6.2.2. <a name='Technicaldetails'></a>Technical details

Add the following sections (as content, not necessarily as headings) and fill with concrete statements.

- Problem statement (placeholder)
- Proposed solution / product pillars (placeholder)
- Success criteria (placeholder)
- Target audience and personas (placeholder)
- Non-goals (explicitly include: multiplayer, cloud saves, mandatory remote telemetry) (placeholder)

####  6.2.3. <a name='Phaserequirements'></a>Phase requirements

- (***Not started***) Define 2 to 4 player personas.
  - GIVEN the MVP is single-player
  - WHEN personas are written
  - THEN at least one persona represents new players and at least one represents experienced 4x players

- (***Not started***) Define MVP scope boundaries.
  - GIVEN the existing system plan list
  - WHEN MVP scope is declared
  - THEN it identifies which systems are required for MVP and which are deferred

###  6.3. <a name='Phase2-Moddingandcontentpipeline'></a>Phase 2 - Modding and content pipeline

***Not started***

####  6.3.1. <a name='Objective-1'></a>Objective

Define modding as a first-class product feature, including an opt-in, decentralised mod discovery model.

####  6.3.2. <a name='Technicaldetails-1'></a>Technical details

Document (at minimum):

- What a "mod" can change (data only vs scripting vs assets) (TBD)
- Mod package and manifest format (TBD)
- Dependency management and version compatibility rules (TBD)
- Load order and conflict resolution strategy (TBD)
- Safety and trust model (TBD)
- Base mod packaging and update strategy (placeholder)
- In-game mod manager UX requirements (placeholder)
- Decentralised registry ("bucket") design constraints and integrity checks (placeholder)

####  6.3.3. <a name='Phaserequirements-1'></a>Phase requirements

- (***Not started***) Define mod manager user stories.
  - GIVEN a player wants to mod the game
  - WHEN they open the mod manager
  - THEN they can install, update, enable, disable, and remove mods with clear feedback

- (***Not started***) Define decentralised registry constraints.
  - GIVEN mod registries are decentralised
  - WHEN the PRD describes discovery
  - THEN it defines how registries are added, how integrity is verified, and how dependency metadata is obtained

###  6.4. <a name='Phase3-Diagnosticsandopt-intelemetry'></a>Phase 3 - Diagnostics and opt-in telemetry

***Not started***

####  6.4.1. <a name='Objective-1'></a>Objective

Define MVP diagnostics (local-first) and post-MVP remote telemetry requirements.

####  6.4.2. <a name='Technicaldetails-1'></a>Technical details

Document:

- Local logging requirements (log location, rotation, retention, export flow) (placeholder)
- Crash dump requirements (what is captured and how users can disable it) (placeholder)
- Required log events (startup, mod loading, save/load, performance counters, errors) (placeholder)
- Opt-in telemetry consent UX and data minimisation rules (placeholder)

####  6.4.3. <a name='Phaserequirements-1'></a>Phase requirements

- (***Not started***) Define a "Create diagnostics bundle" experience.
  - GIVEN a player encounters a problem
  - WHEN they choose to export diagnostics
  - THEN the game creates a local bundle containing logs and relevant metadata with no personal data unless explicitly opted in

###  6.5. <a name='Phase4-MVPverticalslicedefinition'></a>Phase 4 - MVP vertical slice definition

***Not started***

####  6.5.1. <a name='Objective-1'></a>Objective

Define a single end-to-end slice that proves the product is coherent and testable.

####  6.5.2. <a name='Technicaldetails-1'></a>Technical details

Document an MVP vertical slice such as:

- Generate a map
- Place a starting location
- Gather at least one resource
- Recruit or assemble an army
- Trigger an on-map battle
- Save and load
- Verify state restoration

Include explicit acceptance criteria and manual verification steps.

####  6.5.3. <a name='Phaserequirements-1'></a>Phase requirements

- (***Not started***) Define acceptance criteria for the vertical slice.
  - GIVEN a single MVP slice is described
  - WHEN acceptance criteria are listed
  - THEN each criterion is observable and verifiable without guessing intent

###  6.6. <a name='Phase5-Risksandroadmap'></a>Phase 5 - Risks and roadmap

***Not started***

####  6.6.1. <a name='Objective-1'></a>Objective

Make delivery predictable by documenting phased rollout and risks.

####  6.6.2. <a name='Technicaldetails-1'></a>Technical details

Document:

- Phased rollout: MVP (Windows, single-player, local diagnostics, base mod) to later phases (Linux consideration, opt-in telemetry, richer mod ecosystem) (placeholder)
- Key risks: mod safety, version compatibility, decentralised registry integrity, diagnostics storage and privacy (placeholder)

###  6.7. <a name='Phase6-PRDreviewandsign-off'></a>Phase 6 - PRD review and sign-off

***Not started***

####  6.7.1. <a name='Objective-1'></a>Objective

Ensure the PRD is implementable and internally consistent with the system plans.

####  6.7.2. <a name='Technicaldetails-1'></a>Technical details

Review checklist:

- All non-plain-English terms are defined before use.
- All requirements are measurable or have explicit manual verification steps.
- Modding scope is explicit and not ambiguous.
- Diagnostics requirements are explicit for MVP.

##  7. <a name='Seealso'></a>See also

- `docs/plans/4x-game.md` (system-level plans)
- `docs/plans/4x-game/systems/` (gameplay systems)
- `docs/plans/4x-game/technical/` (technical systems)
- `docs/templates/plan-template.md` (documentation template)

##  8. <a name='References'></a>References

- MVP: <https://en.wikipedia.org/wiki/Minimum_viable_product>
- Telemetry: <https://en.wikipedia.org/wiki/Telemetry>
