---
name: planktonsoup-editable-software-diagrams
description: Create or update software-development diagrams in Mermaid (`.mmd` or Markdown code fences) or draw.io (`.drawio`) format. Use when an agent needs human-readable, diffable diagrams for system architecture, service boundaries, runtime flows, sequence diagrams, ERDs, deployment layouts, state machines, dependency maps, integration boundaries, or technical documentation, and the output should remain easy for humans and agents to review and edit later. Stay focused on Mermaid and draw.io only unless the user explicitly asks for another format.
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

1. Inspect the request, the relevant code, nearby docs, and any existing diagrams before choosing a format or abstraction level.
2. Base the diagram on the implementation that actually exists in the project. Prefer real modules, services, tables, queues, topics, endpoints, jobs, environments, and boundaries over guessed placeholders.
3. Use official or high-signal MCP tools when they materially improve accuracy or save time gathering inputs.
4. Reuse the existing format when editing an existing diagram unless the user explicitly asks to migrate it.
5. If the requested diagram type or notation is unclear, research the common convention for that diagram type before inventing a structure.
6. If no format is specified, choose the simplest editable format that preserves the intent.
7. Produce the diagram source directly; do not describe a diagram without creating the file content unless the user asked for concepts only.
8. Verify the rendered structure, not just the text syntax, before finalizing. If a required rule fails, the diagram is not complete.
9. Keep the output diff-friendly and avoid generated noise.

## Non-Negotiable Rules

- Required rules are mandatory. Do not treat them as style preferences or best-effort guidance.
- If a required Mermaid or draw.io rule fails in preview, render validation, or source review, the diagram is not complete.
- Do not trade correctness, readability, or theme-safe rendering away for speed, visual preference, or keeping a fragile layout unchanged.
- When a format-specific rule conflicts with a general preference, the format-specific rule wins.

## Decision Rules

Apply this order:

1. If the user names Mermaid or draw.io, use that format.
2. If an existing target file already uses Mermaid or draw.io, preserve that format.
3. If the output belongs inside Markdown docs or should be easy for developers to view in place, prefer Mermaid hosted in a `.md` file.
4. If exact placement, grouping, swimlanes, or canvas composition matter, prefer draw.io.
5. For most codebase and documentation diagrams, prefer Mermaid because it is lighter to diff and edit.

## Prefer MCP When Useful

Use MCP tools when they are official, already configured, or clearly better than ad hoc inspection.

- Prefer local workspace inspection first when diagramming a codebase that is already present; the diagram should reflect the implemented system, not a guessed architecture.
- Prefer repository or GitHub MCP tools to inspect PRs, changed files, issue context, and remote source artifacts before diagramming a system or change.
- Prefer product-specific official MCP tools when diagramming external systems, APIs, schemas, or platform resources and those tools expose authoritative data.
- Prefer MCP or repository search over guesswork when the diagram depends on actual code structure, service names, schema shape, or deployed resources.
- Prefer local shell/file inspection when the needed context is already in the workspace and MCP would add overhead without improving accuracy.
- Do not invent MCP dependencies in the skill metadata unless the skill actually requires them to function.
- Keep the skill format-agnostic: MCP tools help gather inputs, but the output should still be Mermaid or draw.io source.

## Research Conventions Only When Needed

- If the user clearly names the diagram type and the project already has similar diagrams, follow the existing project pattern.
- If the diagram type is unclear, mixed, or unusual, research the common rendering convention before inventing a custom structure.
- Prefer established technical diagram conventions over novel notation.
- Keep the research lightweight and focused on structure, notation, and common expectations for that diagram family, not on style churn.

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
- Preserve labels, IDs, layout intent, and surrounding documentation structure unless the request requires a broader rework.
- Do not revamp an existing diagram unless the request requires it or the current diagram violates the skill's correctness or readability requirements.
- Keep Mermaid, Markdown, and raw-XML draw.io files in UTF-8 without BOM unless the existing file already uses a different encoding and the user explicitly wants that preserved.
- If an existing `.drawio` file is stored as compressed or encoded diagram content, decode it to raw XML before making source edits so the actual structure can be inspected and changed safely.
- For draw.io, keep files uncompressed unless the file already uses compression or compatibility requires it.
- For Mermaid embedded in Markdown, edit only the targeted fenced block unless the surrounding prose also needs updates.

## Mermaid Rules

- Emit valid Mermaid syntax with a single top-level diagram declaration.
- Use short, stable node IDs and human-readable labels.
- When the request maps to a Mermaid diagram family, use that family directly instead of forcing everything into a flowchart.
- Keep labels concise and parser-safe. Prefer plain phrases over embedded quotes, escaped literals, or dense punctuation.
- Prefer explicit edge labels when transitions or data movement would otherwise be ambiguous.
- Keep connector labels background-transparent. Do not apply draw.io-style filled plates or semi-opaque boxes to Mermaid edge labels.
- Connect edges to concrete nodes. Do not rely on edges targeting `subgraph` IDs or container labels; many renderers draw those as floating or detached.
- Keep the source readable enough for a human to edit without rendering first.

Read [references/mermaid.md](references/mermaid.md) for diagram type selection, label safety rules, CLI validation, and rendering constraints.
Start from [assets/mermaid-doc-template.md](assets/mermaid-doc-template.md) for new Markdown-hosted diagrams.

## Draw.io Rules

- Emit valid `.drawio` XML with `compressed="false"` unless the file already uses compression.
- Prefer a single page unless the user asks for multiple pages.
- Keep labels as plain readable text and keep the XML manually editable.
- Prefer connectors with explicit `source` and `target` shape IDs over loose geometry-only lines.
- Always set an explicit page background color so text readability does not depend on the editor theme.
- Use explicit dark or high-contrast connector stroke and arrow colors that contrast with the page background.
- For edge labels, branch labels, and connector-adjacent annotations, use a dedicated label vertex with a light neutral fill at roughly `70%` `fillOpacity` and `Background Style = None`. Do not leave them fully transparent or rely on inline edge-label background styling alone.
- Treat theme-dependent black label plates or canvas-punch-through transparency as correctness bugs.

Start from [assets/blank.drawio](assets/blank.drawio) when creating a new draw.io file from scratch. Read [references/drawio.md](references/drawio.md) for the minimal structure and editing rules.

## Readability And Styling

When the repository does not enforce a conflicting style, use a high-contrast presentation: dark saturated fills with white text, a small set of semantic color families, visually distinct container outlines versus connector lines, and a legend when more than three semantic colors appear. All text must contrast with its immediate surface. Treat weak contrast as a correctness failure.

Read [references/styling.md](references/styling.md) for the full palette strategy, connector treatment, labeling rules, and layout guidelines.

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
- For new reusable standalone Mermaid files, follow the same heading, context, and naming patterns used by the Markdown template without inventing extra boilerplate.

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

- Prefer local CLI validation first when a suitable tool is installed.
- For Mermaid, probe for `mmdc` in a cross-platform way and use it to render the diagram from the final source or from a temporary extracted Mermaid block before considering the diagram done.
- For draw.io, probe for `drawio`, `draw.io`, or `diagrams.net` in a cross-platform way and use the local export or preview capabilities when available before considering connector or label changes done.
- Probe order should be: shell-resolvable command on `PATH` first, then common package-manager shims and app install locations for the current OS.
- On Windows, also check common npm, Chocolatey, Scoop, and desktop-app locations when the command is not already on `PATH`.
- On macOS, also check common Homebrew locations and standard app paths such as `/Applications` and `~/Applications` when the command is not already on `PATH`.
- On Linux, also check common user-local and system directories such as `~/.local/bin`, `/usr/local/bin`, `/usr/bin`, `/snap/bin`, and AppImage-style install locations when the command is not already on `PATH`.
- If a local diagram CLI exists but its flags vary by install, inspect `--help` first and then run the narrowest render or export command that validates the edited file.
- If no local CLI is available, fall back to preview-based or manual structural validation and state that render validation was unavailable.
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
- In draw.io, verify that raw-XML files contain no comments inside `<diagram>`, no stray text nodes, and only `<mxCell>` children inside `<root>`.
- In draw.io, verify that `<diagram>` contains exactly one `<mxGraphModel>` element and no extra text or tags.
- In draw.io, verify that raw-XML files are saved as UTF-8 without BOM.
- In draw.io, check that routed connectors still attach correctly after moving grouped boxes, containers, or section boundaries.
- In draw.io, verify that connector strokes and arrowheads remain visibly distinct from the page background and are not white or near-white on light pages.
- Verify that all text remains visibly distinct from its local background, fill, or label surface in the rendered preview.
- In draw.io, verify that the page background color is explicitly set and that labels remain readable in both dark and light render modes.
- In draw.io, verify that any transparent non-connector labels use `Background Style = None` and that source-edited transparent labels keep `labelBackgroundColor=none;` when applicable.
- In draw.io, verify that connector labels do not fall back to theme-default black plates or editor-canvas punch-through. If they do, the diagram is not complete until the page/background-style setup and local label treatment are corrected.
- In draw.io, verify that edge labels, branch labels, and other connector-adjacent annotations use dedicated semi-opaque label vertices unless a preview proves the inline edge-label rendering is truly translucent.
- In draw.io, verify that connector-label translucency comes from explicit vertex style settings such as `fillOpacity=70`, not from unsupported or ineffective background-opacity assumptions.
- In draw.io, verify that dedicated connector-label vertices also use `Background Style = None` and do not render against the dark or light editor theme behind the page.
- In draw.io, when a label uses a local fill for readability, verify that the fill is only as strong as needed, typically around `70%` `fillOpacity`, while the text remains fully readable.
- In draw.io, verify that connector-label text remains visibly distinct from both the label fill and the page background; white or near-white text on light label fills is incorrect.
- In draw.io, verify that decision labels and similar edge labels remain attached to the correct branch or connector location after readability fixes; do not move them away from the meaning they annotate just to avoid styling the label.
- In Mermaid, verify that every referenced node ID exists exactly once and that edges render to the intended node after any rename or refactor.
- In Mermaid, verify in a rendered preview that arrowheads visibly meet the intended node or anchor node rather than stopping short or appearing offset.
- In Mermaid, verify that connector labels render with transparent backgrounds rather than filled plates or backfills.
- In Mermaid, verify that intended multiline labels render as actual line breaks rather than literal `\\n` text.
- In Mermaid, verify that `.mmd` files and Markdown files containing Mermaid blocks are saved as UTF-8 without BOM.
- In Mermaid, do a parser-safety pass over every node label and edge label before finalizing. Rewrite labels that contain embedded double quotes, escaped quotes, or code-like delimiter sequences when a simpler phrase preserves the meaning.
- In Mermaid, when `mmdc` is available locally, require a successful render from the final source before treating the diagram as complete.
- In Mermaid, if long or diagonal edges render poorly, shorten the route by re-laying out nodes, adding intermediate anchor nodes, or splitting the diagram instead of accepting a barely attached edge.
- In dense diagrams, verify that containers are instantly distinguishable from connectors by stroke treatment, color, or visual weight; if not, restyle or simplify the diagram.
- Verify that no two meaningful lines overlap on the same trajectory long enough to look like a single line; reroute or separate them if they do.
- If a preview reveals a visually detached connector, mis-anchored label, or ambiguous target, fix it before considering the diagram complete.
- For Mermaid, require a render check in a Mermaid-compatible preview when available; otherwise, do a manual source review that confirms each referenced edge endpoint is a real node ID declared in the file.
- For Mermaid, if a parser or preview reports an error location inside a label, treat the label text as suspect first and simplify it before changing the diagram structure.
- For draw.io, when a local `drawio`, `draw.io`, or `diagrams.net` CLI is available, require a successful local export or preview-oriented validation step before treating connector or label edits as complete.

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
- Derive names and boundaries from the repository's actual implementation, not from idealized architecture language, unless the user explicitly wants a conceptual view.
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
