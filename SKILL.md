---
name: planktonsoup-editable-software-diagrams
description: Create or update software-development diagrams in Mermaid (`.mmd` or Markdown code fences) or draw.io (`.drawio`) format. Use when an agent needs human-readable, diffable diagrams for system architecture, service boundaries, runtime flows, sequence diagrams, ERDs, deployment layouts, state machines, dependency maps, integration boundaries, or technical documentation, and the output should remain easy for humans and agents to review and edit later. Stay focused on Mermaid and draw.io only unless the user explicitly asks for another format.
license: See LICENSE
---

# Editable Diagrams

Make software diagrams in text-first formats. Mermaid = concise, source-controlled. draw.io = layout, positioning, mixed visual.

## Scope

Create/edit:
- Mermaid `.mmd` or Markdown fenced blocks
- draw.io `.drawio` XML

For: architecture, service, flow, sequence, deployment, ERD, state, integration, dependency diagrams.

No PlantUML, Excalidraw, SVG, PNG, Visio, image-gen unless user overrides.

## Workflow

1. Inspect request, code, docs, existing diagrams before picking format or abstraction.
2. Base diagram on actual impl — real modules, services, tables, queues, endpoints, boundaries.
3. Use MCP tools when they improve accuracy or save time. Local workspace first; repo/GitHub MCP for remote context; product MCP for external APIs/schemas. No invented MCP deps.
4. Reuse existing format when editing unless user migrates.
5. Diagram type or notation unclear → research convention before inventing structure.
6. Pick simplest editable format preserving intent when no format specified.
7. Produce diagram source directly — no describe-only unless concepts-only requested.
8. Validate rendered structure, not just text syntax. Required rule fails = diagram incomplete.
9. Keep output diff-friendly.

## Non-Negotiable Rules

- Required rules mandatory, not preferences.
- Format rule fails in preview/render/source review → diagram incomplete.
- No trade of correctness, readability, or theme-safe rendering for speed or layout.
- Format-specific rules override general prefs when conflict.

## Format Selection

Priority:
1. User-named format.
2. Preserve existing file format.
3. Mermaid in Markdown when output belongs in docs.
4. draw.io when exact placement, swimlanes, or canvas composition matter.
5. Default: Mermaid — lighter to diff/edit.

**Migration**: Only when user requests. Preserve labels, grouping intent, edge meaning, terminology. No silent migration during unrelated edits.

## Editing Existing Diagrams

- Read file first; preserve format.
- Smallest coherent change satisfying request.
- Preserve labels, IDs, layout intent, surrounding doc structure.
- No revamp unless request requires or diagram violates correctness/readability.
- Files: UTF-8, no BOM.
- draw.io compressed/encoded content: decode to raw XML before edit.
- Mermaid in Markdown: edit only targeted fenced block unless prose also needs update.

## Mermaid

Single top-level diagram declaration, short stable node IDs, human-readable labels. Match diagram family to request. Labels concise, parser-safe. Edges connect to concrete nodes, not subgraph IDs. Connector labels background-transparent.

Read [references/mermaid.md](references/mermaid.md) for type selection, label safety, CLI validation, rendering constraints.
Start from [assets/mermaid-doc-template.md](assets/mermaid-doc-template.md) for new Markdown-hosted diagrams.

## Draw.io

Valid `.drawio` XML, `compressed="false"`. Explicit `source`/`target` connector IDs. Always set explicit page background color. Visible connector strokes/arrow colors contrast with background. Dedicated semi-opaque label vertices for edge/branch labels.

Read [references/drawio.md](references/drawio.md) for XML structure, editing rules, label treatment, validation.
Start from [assets/blank.drawio](assets/blank.drawio) for new diagrams.

## Styling

High-contrast: dark saturated fills + white text, small semantic color families, distinct container outlines vs connector lines, legend when >3 semantic colors. All text must contrast with immediate surface — weak contrast = correctness failure.

Read [references/styling.md](references/styling.md) for palette, connector treatment, labeling, layout.

## Output Conventions

- Mermaid: embed in `.md` when devs view in docs; standalone `.mmd` when reused, large, or already exists as `.mmd`.
- Mermaid: wrap in ` ```mermaid ` fenced block with short heading/caption above.
- draw.io: `.drawio` extension.
- Replacing ASCII/prose descriptions: preserve original meaning.
- Create/edit doc artifact — no result left in chat only.
- Update nearest `README.md` or diagram index when adding/renaming persisted diagrams.
- Place in established dirs (`diagrams/`, `docs/diagrams/`, etc.) or alongside supporting docs. Preserve existing placement.
- Descriptive file names (`auth-sequence.mmd`, `deployment.drawio`). No generic names (`diagram1.drawio`).
- Markdown-hosted: concise heading + 1–2 context lines above, one primary diagram per section, small TOC if multiple diagrams.
- No repo doc depend on external viewer links unless user requests.
- Start from [assets/mermaid-doc-template.md](assets/mermaid-doc-template.md) for new diagram Markdown pages.

## Validation

Before finalizing, verify:

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

Use local CLI tools (`mmdc`, `drawio`) when available. Probe cross-platform before fallback to preview or manual review.

## Request Defaults

When underspecified, infer smallest useful diagram:
- Architecture → main runtime components + edges
- Workflow → major states + decisions
- Data model → entities + cardinality
- Sequence → main actors, messages, key responses/failures
- Deployment → runtime units, boundaries, key deps

Real code names over placeholders. Show system boundaries when crossing services or trust zones. Distinguish internal from external. Single abstraction level: system context, container/service, runtime interaction, deployment topology, or data model.

## Quality Bar

- Answer actual question — no draw-everything.
- Optimize readability at a glance.
- Terminology consistent with surrounding code/docs.
- Abstraction level explicit; no mixed levels unless request requires.
- Split crowded diagrams.
- Trust authoritative MCP sources over local assumptions.
- No decorative complexity — artifact easier to maintain after edit.
- Visually detached arrows, labels, connectors = correctness bugs.
