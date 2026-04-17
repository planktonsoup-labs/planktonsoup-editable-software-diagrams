---
name: planktonsoup-editable-software-diagrams
description: Create/update software diagrams in Mermaid (.mmd or MD fences) or draw.io (.drawio). Use for arch, service, flow, seq, ERD, deploy, state, dep diagrams. Output stays editable, diffable, human-reviewable. Mermaid + draw.io only unless user overrides.
license: See LICENSE
---

# Editable Diagrams

Text-first diagrams. Mermaid = concise, source-controlled. draw.io = layout, position, mixed visual.

## Scope

Create/edit:
- Mermaid `.mmd` or MD fenced blocks
- draw.io `.drawio` XML

For: arch, service, flow, seq, deploy, ERD, state, integration, dep diagrams.

No PlantUML, Excalidraw, SVG, PNG, Visio, image-gen unless user overrides.

## Workflow

1. Inspect req/code/docs/existing diagrams → pick format + abstraction.
2. Base on actual impl — real modules, services, tables, queues, endpoints, boundaries.
3. MCP tools when they improve accuracy. Local workspace first; repo/GitHub MCP for remote ctx; product MCP for external APIs/schemas. No invented deps.
4. Editing → reuse existing format unless user migrates.
5. Type/notation unclear → research convention before inventing structure.
6. Simplest editable format preserving intent when no format specified.
7. Output diagram source directly — no describe-only unless concepts-only requested.
8. Validate rendered structure, not just text syntax. Required rule fails → diagram incomplete.
9. Diff-friendly output.

## Non-Negotiable Rules

- Required rules = mandatory, not preferences.
- Format rule fails in preview/render/source review → diagram incomplete.
- No trade of correctness/readability/theme-safe rendering for speed/layout.
- Format-specific rules override general prefs on conflict.

## Format Selection

Priority:
1. User-named format.
2. Preserve existing file format.
3. Mermaid in MD when output belongs in docs.
4. draw.io when exact placement, swimlanes, canvas composition matter.
5. Default: Mermaid — lighter to diff/edit.

**Migration**: User-requested only. Preserve labels, grouping, edge meaning, terminology. No silent migration during unrelated edits.

## Editing Existing Diagrams

- Read file first; preserve format.
- Smallest coherent change satisfying req.
- Preserve labels, IDs, layout intent, surrounding doc structure.
- No revamp unless req requires or diagram violates correctness/readability.
- Files: UTF-8, no BOM.
- draw.io compressed/encoded → decode to raw XML before edit.
- Mermaid in MD: edit only targeted fenced block unless prose also needs update.

## Mermaid

Single top-level diagram decl, short stable node IDs, human-readable labels. Match diagram family to req. Labels concise, parser-safe. Edges → concrete nodes, not subgraph IDs. Connector labels bg-transparent.

Read [references/mermaid.md](references/mermaid.md) for type selection, label safety, CLI validation, rendering constraints.
Start from [assets/mermaid-doc-template.md](assets/mermaid-doc-template.md) for new MD-hosted diagrams.

## Draw.io

Valid `.drawio` XML, `compressed="false"`. Explicit `source`/`target` connector IDs. Always set explicit page bg color. Connector strokes/arrow colors contrast with bg. Dedicated semi-opaque label vertices for edge/branch labels.

Read [references/drawio.md](references/drawio.md) for XML structure, editing rules, label treatment, validation.
Start from [assets/blank.drawio](assets/blank.drawio) for new diagrams.

## Styling

High-contrast: dark saturated fills + white text, small semantic color families, distinct container outlines vs connector lines, legend when >3 semantic colors. All text contrasts with immediate surface — weak contrast = correctness failure.

Read [references/styling.md](references/styling.md) for palette, connector treatment, labeling, layout.

## Output Conventions

- Mermaid: embed in `.md` when devs view in docs; standalone `.mmd` when reused, large, or already `.mmd`.
- Mermaid: wrap in ` ```mermaid ` fenced block, short heading/caption above.
- draw.io: `.drawio` ext.
- Replacing ASCII/prose descriptions: preserve original meaning.
- Create/edit doc artifact — no result left in chat only.
- Update nearest `README.md` or diagram index when adding/renaming persisted diagrams.
- Place in established dirs (`diagrams/`, `docs/diagrams/`) or alongside supporting docs. Preserve existing placement.
- Descriptive filenames (`auth-sequence.mmd`, `deployment.drawio`). No generic names (`diagram1.drawio`).
- MD-hosted: concise heading + 1–2 ctx lines above, one primary diagram per section, small TOC if multiple diagrams.
- No repo doc depend on external viewer links unless user requests.
- Start from [assets/mermaid-doc-template.md](assets/mermaid-doc-template.md) for new diagram MD pages.

## Validation

Before finalizing:

**Cross-format:**
- Every edge starts/ends on actual rendered node or shape.
- Arrowheads visibly touch intended targets in preview.
- All text contrasts with immediate surface.
- No overlapping lines reading as single line.
- Containers visually distinct from connectors.
- Diagram too crowded to verify → simplify or split.
- Preview shows detached connectors, mis-anchored labels, ambiguous targets → fix before completing.

**Format-specific validation** in each reference:
- [references/mermaid.md](references/mermaid.md) — label safety, node ID integrity, `mmdc` CLI validation
- [references/drawio.md](references/drawio.md) — XML safety checklist, CLI validation, connector/label rendering

Local CLI (`mmdc`, `drawio`) when available. Probe cross-platform → fallback to preview/manual review.

## Request Defaults

Underspecified → infer smallest useful diagram:
- Architecture → main runtime components + edges
- Workflow → major states + decisions
- Data model → entities + cardinality
- Sequence → main actors, messages, key responses/failures
- Deployment → runtime units, boundaries, key deps

Real code names over placeholders. Show system boundaries when crossing services/trust zones. Distinguish internal from external. Single abstraction level.

## Quality Bar

- Answer actual question — no draw-everything.
- Readability at a glance.
- Terminology consistent with surrounding code/docs.
- Abstraction level explicit; no mixed levels unless req requires.
- Split crowded diagrams.
- Trust authoritative MCP sources over local assumptions.
- No decorative complexity — artifact easier to maintain after edit.
- Visually detached arrows/labels/connectors = correctness bugs.
