# AGENTS

Maintain this skill as a small, standards-aligned agent skill. Optimize for clarity, low ambiguity, and editable outputs.

## Purpose

This skill exists to help agents create and update software-development diagrams in:

- Mermaid
- draw.io / diagrams.net

The skill should stay focused on those formats. Do not broaden scope to image-generation, PlantUML, Excalidraw, Visio, or other diagram systems unless explicitly requested.

## Source Of Truth

- `SKILL.md` is the primary behavior contract.
- `references/styling.md` contains only general visual guidance shared across formats.
- `references/mermaid.md` contains Mermaid-specific syntax, validation, and rendering rules.
- `references/drawio.md` contains draw.io-specific XML, rendering, and validation rules.
- `README.md` explains the directory to humans and should stay product-neutral.
- `agents/openai.yaml` is runtime metadata for environments that use that file convention.

When a rule is format-specific, keep it out of `styling.md` and put it in the relevant format reference.

## Invariants

- Keep the skill name consistent as `planktonsoup-editable-software-diagrams` wherever the skill name is described.
- Keep product wording generic. Avoid vendor-specific prose in documentation unless a filename or compatibility surface requires it.
- Keep Mermaid and draw.io rules clearly separated.
- Keep required behaviors explicit. If something is non-negotiable, write it as a requirement, not a preference.
- Keep outputs editable, diff-friendly, and human-reviewable.
- Keep changes small and coherent. Do not rewrite the whole skill when a localized change is enough.

## Maintenance

- Prefer editing existing files over adding new ones.
- Do not add example artifacts, generated diagrams, screenshots, or extra folders unless they are clearly necessary.
- Do not leave broken references to missing files, deleted assets, or old naming.
- Do not duplicate the same rule in multiple places unless the duplication materially improves compliance.
- If a rule is repeated, one copy should be the canonical detailed version and the other should remain short.
- Keep README language aligned with the current implementation and file layout.
- Keep assets minimal. Templates and starter files are acceptable only when they are actively useful.

## Rule Placement

- Put workflow, decision rules, scope, and verification expectations in `SKILL.md`.
- Put cross-format styling principles in `references/styling.md`.
- Put Mermaid parser, label, preview, and rendering constraints in `references/mermaid.md`.
- Put draw.io page background, label vertex, XML, and dark/light rendering constraints in `references/drawio.md`.

If you are unsure where a new instruction belongs, place it in the most specific file that fully owns that behavior.

## Validation

- Re-read any file you touch and remove stale or contradictory wording.
- Check that referenced files and assets actually exist.
- Check that format-specific rules did not leak into `references/styling.md`.
- Keep required validation and correctness checks intact unless you are intentionally improving them.

## Avoid

- Product-specific marketing language.
- Broadening the skill beyond software diagrams in Mermaid or draw.io.
- Softening hard requirements into optional guidance.
- Adding noisy metadata, extra boilerplate, or speculative integrations.
- Reorganizing the directory without a clear maintenance benefit.
