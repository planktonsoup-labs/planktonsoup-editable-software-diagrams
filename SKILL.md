---
name: planktonsoup-editable-software-diagrams
description: Create or update software-development diagrams in Mermaid (`.mmd` or Markdown code fences) or draw.io (`.drawio`) format. Use when Codex needs human-readable, diffable diagrams for system architecture, service boundaries, runtime flows, sequence diagrams, ERDs, deployment layouts, state machines, dependency maps, integration boundaries, or technical documentation, and the output should remain easy for humans and agents to review and edit later. Stay focused on Mermaid and draw.io only unless the user explicitly asks for another format.
license: See LICENSE
---

# Editable Diagrams

## Overview

Create software-development diagrams in text-first formats that work well in agentic workflows. Prefer Mermaid for concise source-controlled technical diagrams and prefer draw.io when architecture or deployment views need manual positioning, richer layout control, or mixed visual elements.

## Scope

Use this skill only for creating or editing:

- Mermaid source in standalone `.mmd` files
- Mermaid inside Markdown fenced blocks
- draw.io / diagrams.net `.drawio` XML files

Focus on technical documentation artifacts such as:

- system and service architecture diagrams
- request, event, and data flows
- sequence diagrams for runtime interactions
- deployment and infrastructure topology views
- entity-relationship and domain model diagrams
- state diagrams for business or runtime lifecycles
- integration maps, module boundaries, and dependency views

Do not switch to PlantUML, Excalidraw, SVG, PNG, Visio, or image-generation workflows unless the user explicitly overrides this skill's scope.

## Typical Requests

- "Create a Mermaid sequence diagram for our login flow."
- "Add Redis to this existing draw.io architecture diagram."
- "Turn this ASCII workflow into a Mermaid flowchart in the README."
- "Document the order lifecycle as a state diagram."
- "Make a draw.io deployment diagram from this Kubernetes manifest."
- "Show how the web app, API, queue, and worker interact."
- "Create an ERD for these tables."
- "Add the new payment provider boundary to the system context diagram."

## Workflow

1. Inspect the request and any existing diagram files before choosing a format.
2. Use official or high-signal MCP tools when they materially improve accuracy or save time gathering diagram inputs.
3. Reuse the existing format when editing an existing diagram unless the user explicitly asks to migrate it.
4. If no format is specified, choose the simplest editable format that preserves the intent.
5. Produce the diagram source directly; do not describe a diagram without creating the file content unless the user asked for concepts only.
6. Verify the rendered structure, not just the text syntax, before finalizing.
7. Keep the output diff-friendly and avoid generated noise.

## Decision Rules

Apply this order:

1. If the user names Mermaid or draw.io, use that format.
2. If an existing target file already uses Mermaid or draw.io, preserve that format.
3. If the output belongs inside Markdown docs or should be easy for developers to view in place, prefer Mermaid hosted in a `.md` file.
4. If exact placement, grouping, swimlanes, or canvas composition matter, prefer draw.io.
5. For most codebase and documentation diagrams, prefer Mermaid because it is lighter to diff and edit.

## Prefer MCP When Useful

Use MCP tools when they are official, already configured, or clearly better than ad hoc inspection.

- Prefer repository or GitHub MCP tools to inspect PRs, changed files, issue context, and remote source artifacts before diagramming a system or change.
- Prefer product-specific official MCP tools when diagramming external systems, APIs, schemas, or platform resources and those tools expose authoritative data.
- Prefer MCP or repository search over guesswork when the diagram depends on actual code structure, service names, schema shape, or deployed resources.
- Prefer local shell/file inspection when the needed context is already in the workspace and MCP would add overhead without improving accuracy.
- Do not invent MCP dependencies in the skill metadata unless the skill actually requires them to function.
- Keep the skill format-agnostic: MCP tools help gather inputs, but the output should still be Mermaid or draw.io source.

## Choose The Format

Default to Mermaid when:

- The user asks for a flowchart, sequence diagram, state diagram, class diagram, ERD, dependency view, request flow, or simple architecture diagram.
- The diagram should live inside Markdown documentation, a design note, or a nearby README, or in a standalone `.mmd` file when separation is cleaner.
- Fast iteration and readable text diffs matter more than exact positioning.

Default to draw.io when:

- The user asks for a `.drawio` file or mentions draw.io, diagrams.net, or XML.
- The diagram needs manual placement, freeform canvas layout, swimlanes, grouped containers, infrastructure zones, or mixed annotations that would be awkward in Mermaid.
- The repository already stores adjacent diagrams as `.drawio`.

If both are plausible and the user did not decide, prefer Mermaid for simple structured diagrams and draw.io for layout-heavy diagrams.

## Migration Rules

- Migrate Mermaid to draw.io only when the user asks for richer manual layout or a `.drawio` artifact.
- Migrate draw.io to Mermaid only when the diagram is structurally simple enough to preserve meaning without hand-tuned placement.
- When migrating, preserve labels, grouping intent, edge meaning, and file-local terminology.
- Do not silently migrate formats as part of an unrelated edit.

## Edit Existing Diagrams

- Read the existing file first and preserve the current format.
- Make the smallest coherent change that satisfies the request.
- Preserve labels, IDs, and layout intent unless the request requires a broader rework.
- For draw.io, keep files uncompressed when possible so diffs stay readable.
- For Mermaid embedded in Markdown, edit only the targeted fenced block unless the surrounding prose also needs updates.

## Mermaid Rules

- Emit valid Mermaid syntax with a single top-level diagram declaration.
- Use short, stable node IDs and human-readable labels.
- Favor vertical or left-to-right layouts only when they improve readability.
- Keep labels concise; move long explanations outside the diagram when possible.
- When the request maps to a Mermaid diagram family, use that family directly instead of forcing everything into a flowchart.
- Prefer explicit edge labels when transitions or data movement would otherwise be ambiguous.
- Keep the source readable enough that a human can edit it without rendering first.
- Connect edges to concrete nodes whenever possible. Do not rely on edges targeting `subgraph` IDs or container labels for important relationships, because many Mermaid renderers draw those connectors as floating or visually detached.
- When you need to show that a route table, policy, ACL, or shared control applies to a subnet group or container, anchor the edge to a real node inside that container or add a dedicated anchor node inside the container instead of connecting to the container itself.
- Prefer fewer, shorter cross-diagram connectors when labels or arrows start landing far from their intended shapes. Split crowded diagrams or introduce local anchor nodes instead of stretching one edge across multiple containers.
- Treat Mermaid edges that appear to stop short of the target, miss the target visually, or terminate ambiguously as correctness bugs that require a layout or structure change.

Read [references/mermaid.md](references/mermaid.md) when choosing diagram types or syntax patterns.
Start from [assets/mermaid-doc-template.md](assets/mermaid-doc-template.md) for new Markdown-hosted diagrams and [assets/standalone-diagram-template.mmd](assets/standalone-diagram-template.mmd) for new standalone Mermaid files.

## Draw.io Rules

- Emit valid `.drawio` XML with `compressed="false"` unless the file already uses compression.
- Prefer a single page unless the user asks for multiple pages.
- Use a consistent coordinate grid and leave reasonable spacing between shapes.
- Use simple built-in shapes and edge styles before introducing more complex styling.
- Keep labels in the XML as plain readable text; avoid unnecessary metadata.
- Keep the XML manually editable; avoid noisy style churn unless a style change is part of the request.
- Prefer connectors with explicit `source` and `target` shape IDs over loose geometry-only lines.
- When a connector must land on a specific shape, use concrete endpoints or stable entry/exit anchoring instead of relying on approximate placement.

Start from [assets/blank.drawio](assets/blank.drawio) when creating a new draw.io file from scratch. Read [references/drawio.md](references/drawio.md) for the minimal structure and editing rules.

## Readability And Styling

When the repository does not already enforce a conflicting diagram style, prefer a high-contrast presentation that remains readable in editors, rendered views, and diffs.

- Use dark, saturated fills with white text for primary nodes.
- Use darker strokes than fills so shapes remain distinct.
- Reserve color families for semantic roles rather than decorative variation.
- Use lighter tinted containers or section backgrounds when grouping related areas.
- Make containers, section boxes, and grouping outlines visually distinct from connectors.
- Prioritize visual differentiation between containment and flow in dense diagrams, even over decorative consistency.
- Prefer container borders that are thicker, lighter, quieter, or differently colored than arrows so grouping structure does not read like another flow line.
- Add a legend whenever the diagram uses more than three semantic colors.
- Give every diagram a clear title and a short subtitle or context line explaining what it shows.
- Use bold labels for normal nodes and containers; use regular-weight text only for code-like snippets or secondary detail.
- Use solid arrows for required flows and dashed arrows or borders for optional, pluggable, or extensible relationships.
- Avoid giving arrows and area boundaries the same line weight, dash pattern, and color in dense diagrams.
- Avoid routing multiple meaningful lines along the same visual trajectory when they could be mistaken for one line.
- Keep page or canvas width moderate so the diagram fits comfortably in common documentation views.
- If containers and connectors still read as the same texture after styling, reduce density or split the diagram rather than accepting the ambiguity.
- If two connectors or a connector and a boundary overlap for a meaningful stretch, reroute, offset, or split the diagram instead of accepting the overlap.

Read [references/styling.md](references/styling.md) when applying a default visual language.

## Output Conventions

- For Mermaid that developers should view directly in repository docs, prefer embedding it in a nearby `.md` file.
- Use standalone `.mmd` files when the diagram is reused across docs, is large enough to distract from prose, or already exists as `.mmd`.
- For Markdown docs, wrap Mermaid in fenced code blocks using ` ```mermaid ` and keep a short heading or caption immediately above the block.
- For draw.io, save with the `.drawio` extension.
- When replacing an informal ASCII diagram or prose-only description, preserve the original meaning and add a short note only if the migration changes notation.
- If the user asks for "a diagram" without naming a destination file, create or edit the most obvious documentation artifact instead of leaving the result only in chat.
- When adding or renaming a persisted diagram in documentation, update the nearest relevant `README.md` or diagram index if the repository uses one.

## Diagram Locations

Choose the storage location based on the repository's existing documentation layout.

- If the project already has a dedicated diagram directory such as `diagrams/`, `docs/diagrams/`, or a feature-local diagrams folder, prefer placing new persisted diagrams there.
- If Mermaid is embedded directly in a documentation page, keep it in that `.md` file even when a diagrams folder exists.
- If a standalone diagram explains multiple documents or features, prefer the shared diagrams directory and link to it from nearby docs.
- Preserve existing naming and placement conventions instead of creating a parallel diagram structure.
- When a diagrams folder exists, update its nearest `README.md`, index page, or linking document so developers can discover the new artifact.
- Prefer [assets/standalone-diagram-template.mmd](assets/standalone-diagram-template.mmd) when creating a new reusable Mermaid file in a shared diagrams folder.

## Markdown Hosting

When Mermaid is the chosen format, prefer one of these documentation patterns:

- Add the diagram to the nearest feature `README.md` when it explains that feature directly.
- Add the diagram to a focused design note such as `architecture.md`, `runtime-flow.md`, or `data-model.md` when the diagram needs more explanation.
- Link to standalone `.mmd` files from Markdown when the same diagram is referenced from multiple places.

For Markdown-hosted diagrams:

- Add a concise heading and one or two lines of context above the diagram.
- Keep one primary diagram per section.
- If the file contains several diagrams, add a small index or table of contents near the top.
- Prefer placing the Mermaid block close to the text it explains rather than creating a disconnected diagram dump.
- Prefer [assets/mermaid-doc-template.md](assets/mermaid-doc-template.md) when creating a new diagram-focused Markdown page from scratch.

## Design-Time Preview

Use online or app-based viewers as optional design-time aids, not as the long-term source of truth.

- Preview Mermaid in a Mermaid-compatible live editor when checking syntax, layout, or readability during authoring.
- Preview `.drawio` files in diagrams.net or another draw.io-compatible editor when refining placement, sizing, connectors, or grouping.
- Save the final diagram source back into the repository as Markdown-embedded Mermaid, standalone `.mmd`, or `.drawio`.
- Do not make repository documentation depend on external viewer links unless the user explicitly asks for them.
- Prefer repository-native viewing first: Markdown-hosted Mermaid for easy reading in docs, and in-repo `.drawio` files for editable canvas diagrams.
- Use preview tools to validate or refine the diagram, then preserve the final artifact in a diffable form inside the repo.
- For Mermaid, treat preview inspection as mandatory whenever long edges, subgraphs, nested groups, or dense routing could make an endpoint visually unclear.
- For draw.io, treat preview inspection as mandatory whenever connectors, containers, or long routed edges are added or changed.

## Verification Checklist

Before finalizing a diagram, verify these points explicitly:

- Every edge starts and ends on an actual rendered node or shape, not just a container intent.
- Arrowheads visibly touch their intended targets in the preview; if they do not, re-anchor the edge to a concrete node or introduce a local anchor node.
- Subgraph or container relationships are represented with nearby anchor nodes, labels, or notes when direct container-to-container edges render poorly.
- Long dashed control-flow edges do not cut across the page so far that labels drift away from the target they describe.
- Multiline labels remain readable and do not push connector endpoints into awkward positions.
- If the diagram is crowded enough that verification is ambiguous, simplify it or split it into two focused diagrams.
- In draw.io, verify that each changed edge has the intended `source` and `target` IDs and does not rely only on freehand coordinates.
- In draw.io, check that routed connectors still attach correctly after moving grouped boxes, containers, or section boundaries.
- In Mermaid, verify that every referenced node ID exists exactly once and that edges render to the intended node after any rename or refactor.
- In Mermaid, verify in a rendered preview that arrowheads visibly meet the intended node or anchor node rather than stopping short or appearing offset.
- In Mermaid, if long or diagonal edges render poorly, shorten the route by re-laying out nodes, adding intermediate anchor nodes, or splitting the diagram instead of accepting a barely attached edge.
- In dense diagrams, verify that containers are instantly distinguishable from connectors by stroke treatment, color, or visual weight; if not, restyle or simplify the diagram.
- Verify that no two meaningful lines overlap on the same trajectory long enough to look like a single line; reroute or separate them if they do.
- If a preview reveals a visually detached connector, mis-anchored label, or ambiguous target, fix it before considering the diagram complete.
- For Mermaid, prefer a render check in a Mermaid-compatible preview when available; otherwise, do a manual source review that confirms each referenced edge endpoint is a real node ID declared in the file.

## File Naming

- Use descriptive names such as `auth-sequence.mmd`, `order-lifecycle.mmd`, or `deployment.drawio`.
- When adding a new doc-adjacent diagram, keep the name aligned with the nearby feature or document section.
- When using a dedicated diagrams folder, keep filenames specific enough to stand on their own outside the original doc context.
- Avoid generic names like `diagram1.drawio` unless the repository already uses that convention.

## Request Shaping

If the request is underspecified, infer the smallest useful diagram:

- For "architecture diagram," show the main runtime components and their edges.
- For "workflow," show the major states and decisions, not every implementation detail.
- For "data model," show entities and cardinality, not every column unless asked.
- For "sequence diagram," show the main actors, messages, and important responses or failures.
- For "deployment diagram," show the main runtime units, network or trust boundaries, and key dependencies.
- For "integration diagram," show internal vs external systems and the contracts or flows between them.

## Software Diagram Defaults

Unless the user asks for a different level of abstraction:

- Prefer naming components after real code modules, services, jobs, tables, topics, or APIs.
- Show system boundaries explicitly when crossing repositories, services, clouds, or trust zones.
- Distinguish internal components from external providers.
- Show only the level of detail needed for the current engineering question.
- Favor one of these common abstraction levels: system context, container/service view, runtime interaction, deployment topology, or data model.

## Quality Bar

- Make the diagram answer the user's actual question instead of drawing every possible component.
- Optimize for readability at a glance.
- Keep terminology consistent with surrounding code and docs.
- Make the abstraction level explicit and avoid mixing implementation detail with high-level architecture unless the request needs both.
- If a diagram would become crowded, split it into two focused diagrams instead of cramming everything into one.
- If MCP-derived facts conflict with local assumptions, trust the authoritative source and reflect that in the diagram.
- Avoid decorative complexity. The artifact should be easier to maintain after the edit than before it.
- Treat visually detached arrows, labels, or connectors as correctness bugs, not cosmetic polish items.
