# Requirements documentation scaffolding

## Overview

The plans under `docs/plans` describe the intended features of the game, but many of them are currently partial and inconsistent.

This document is an implementation plan for scaffolding a consistent, complete set of requirements documents by:

- Standardising every plan document to the structure in `docs/templates/plan-template.md`.
- Ensuring each plan contains clear requirements and a step-by-step implementation guide.
- Ensuring terms and acronyms are defined before they are used.
- Ensuring links between plans are correct and remain stable.

Scope:

- In scope: `docs/plans/4x-game.md` and everything under `docs/plans/4x-game/`.
- Out of scope: code changes and non-plan documentation outside `docs/plans`.

## Definition of Terms

| Term | Meaning | Reference |
| ---- | ------- | --------- |
| Acceptance criteria | A checklist of observable outcomes that demonstrate the plan has been implemented correctly. | <https://en.wikipedia.org/wiki/Acceptance_testing> |
| Anchor link | A link to a heading within a markdown document, such as `#overview`. | <https://www.markdownguide.org/basic-syntax/#links> |
| Given/When/Then | A structured way of writing requirements as “Given a precondition, when an event happens, then an outcome occurs”. | <https://cucumber.io/docs/gherkin/reference/> |
| Phase | A named chunk of work within a plan, used to break a large feature into implementable parts. | (Project convention; see `docs/templates/plan-template.md`) |
| Requirement status | A short label such as “Not started” or “In development” to communicate progress. | (Project convention; see `docs/templates/plan-template.md`) |
| Wave Function Collapse | A procedural map-generation technique that assembles tiled output by applying adjacency constraints. | <https://en.wikipedia.org/wiki/Wave_function_collapse_algorithm> |

## Requirements

### Documentation structure requirements

1. Every plan document under `docs/plans/4x-game/` MUST follow the section ordering and headings from `docs/templates/plan-template.md`.
2. Every plan document MUST include a table of contents with working links to all top-level sections and any child documents.
3. Every plan document MUST include a “Feature status” section with one of the statuses listed in the template.
4. Every plan document MUST include a “Definition of terms” table and define all non-plain-English terms before they are used.
5. Every plan document MUST include:
   - Feature-level requirements written in Given/When/Then form.
   - A phased breakdown for complex features.
   - Testing guidance.

### Link and naming requirements

6. Existing plan file paths MUST remain stable unless there is a strong reason to rename them.
7. If a plan is renamed or moved, then all inbound links MUST be updated in the same change.
8. Plan titles, file names, and folder names MUST be consistent, predictable, and reflect the table of contents hierarchy.

### Completeness requirements

9. For each plan, requirements MUST be specific enough that a developer can implement the feature without having to infer core behaviour.
10. For each plan, the implementation guide MUST list concrete steps, including where code changes would be made (paths and components), without actually making code edits.
11. For each plan, testing MUST include at least:
    - Unit test ideas.
    - Integration test ideas (when applicable).
    - Manual verification steps (when visual or gameplay validation is required).

## Implementation Steps

1. **Inventory the current plan set**
   - List every markdown file under `docs/plans/4x-game.md` and `docs/plans/4x-game/`.
   - For each file, record whether it already contains the template sections.
   - Identify missing child documents referenced by links (broken or not yet created).

2. **Agree on a standard “minimum complete plan”**
   - Use `docs/templates/plan-template.md` as the canonical structure.
   - Define what “good enough to implement” means for:
     - A small plan (single file).
     - A large plan (parent file plus child documents).

3. **Normalize top-level plan: `docs/plans/4x-game.md`**
   - Ensure it matches the template headings.
   - Ensure the table of contents is complete and matches the folder structure.
   - Ensure “See also” includes links to all child plan documents.

4. **Normalize each child plan document**
   For each file under `docs/plans/4x-game/`:
   - Copy missing sections from `docs/templates/plan-template.md`.
   - Move any existing content into the correct sections.
   - Expand “Definition of terms” into a table and add references.
   - Rewrite requirements into Given/When/Then form.
   - Break the implementation guide into phases if the feature is large.

5. **Create missing child documents (only when linked from the table of contents)**
   - For any linked plan file that does not exist:
     - Create it using `docs/templates/plan-template.md`.
     - Give it an Overview and a minimal set of requirements.
     - Add a clear “Not started” status.

6. **Standardise wording and remove ambiguity**
   - Replace vague statements (“should be robust”, “should be fast”) with measurable intent (“must tolerate partial writes by writing to a temporary file then renaming”).
   - Avoid acronyms; if an acronym is necessary, define it in the “Definition of terms” table.

7. **Add cross-plan integration notes**
   - For each plan, add a short subsection in “Architectural considerations and constraints” describing:
     - What other systems it depends on.
     - What events or data it produces for other systems.
     - Any performance-sensitive paths.

8. **Validate links and formatting**
   - Run `scripts/check-doc-links.ps1`.
   - Run `npx markdownlint-cli **/*.md`.
   - Fix any broken links and markdown issues in the plans.

9. **Review for implementability**
   - Perform a doc review pass with the standard:
     - A developer can implement from the plan without guessing core behaviour.
     - Each requirement can be mapped to tests.
     - Terms are defined before use.

## Implementation Considerations

- **Readability and consistency**
  - Prefer shorter sentences and concrete wording.
  - Keep section headings exactly as in the template to avoid fragmenting conventions.

- **Reliability of documentation links**
  - Avoid renaming files and headings unnecessarily because anchor links are brittle.
  - When headings must change, update all inbound links.

- **Testability and test coverage**
  - Requirements should be written so each one can be validated by at least one test or manual verification step.
  - If a requirement cannot be tested, document how it will be verified.

- **Performance considerations**
  - Plans should call out any parts expected to run every frame (or very frequently) and recommend avoiding unnecessary allocations.
  - When performance requirements exist, include suggested benchmarks (even if not implemented yet).

- **Impact on future features**
  - Document extension points (for example, adding new resource types, new factions, new unit types).
  - Prefer data-driven approaches where the game design is expected to change frequently.

- **Incremental delivery**
  - Complete documentation in small batches (one plan or one folder at a time) to avoid massive review diffs.

## Testing

These are documentation verification steps (not code tests):

1. Link validation: run `scripts/check-doc-links.ps1` and fix any reported missing files or broken relative links.
2. Markdown style validation: run `npx markdownlint-cli **/*.md` and fix any issues in `docs/plans`.
3. Human review checklist (for each plan):
   - All non-plain-English terms used in the plan appear in “Definition of Terms”.
   - Requirements are written in Given/When/Then form.
   - Implementation steps are ordered and actionable.
   - Testing section lists unit, integration (if applicable), and manual verification steps.

## See also

- `docs/templates/plan-template.md` (canonical structure)
- `docs/plans/4x-game.md` (top-level plan)
