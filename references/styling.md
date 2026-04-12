# Diagram Styling Reference

Use this default visual language when the repository does not already provide a stronger diagram convention.

## Goals

- Maximize readability in rendered diagrams and raw source review
- Preserve semantic meaning through consistent color use
- Keep diagrams maintainable by humans and agents
- Make containment and flow visually distinct at a glance, especially in dense technical diagrams

## Default Palette Strategy

Use dark, high-contrast node fills with white text. Reuse a small number of semantic color families consistently:

- green for user-owned logic, application code, or product-specific components
- blue for framework, platform, or infrastructure components
- orange for adapters, boundaries, or configuration-oriented pieces
- red for provider-specific plugins, warnings, or security-sensitive elements
- purple for runtime services or cross-cutting concerns
- blue-grey or slate tones for neutral labels, legends, and non-primary supporting elements

For containers or background sections, use lighter tints of the same semantic family with a darker outline.
Do not style containers and connectors as the same kind of line; grouping boundaries should read as structural framing, not as another arrow.
Treat this distinction as a priority, not an optional polish pass.

## Labeling

- Include a clear diagram title.
- Include a short subtitle, caption, or nearby context sentence explaining scope.
- Prefer bold labels for major nodes and groups.
- Reserve regular text for small annotations, examples, or code-like snippets.

## Connectors

- Use visible arrowheads and at least medium-weight connector strokes.
- Use solid connectors for normal required interactions.
- Use dashed connectors or dashed borders for optional, replaceable, or extensible parts.
- Keep connector color and stroke treatment distinct from container or section outlines.
- In dense diagrams, prefer darker or more directional arrow styling and quieter container borders so flows remain legible.
- If dense diagrams still look like a field of similar lines, simplify the structure or split the diagram instead of trying to solve it with more lines.

## Layout

- Keep diagrams compact enough for common doc widths.
- Favor one dominant flow direction.
- Leave enough whitespace that later edits do not force immediate re-layout.
- Add a legend when more than three semantic colors are in play.

## Applying In Mermaid

- Prefer `classDef` blocks or inline style directives only when the extra styling clearly improves readability.
- Keep the number of classes small and tied to semantic roles.
- Do not over-style if the host Markdown renderer has limited Mermaid support.
- When using subgraphs or styled grouping boxes, keep their border treatment quieter than the main arrows so readers can distinguish containment from flow.
- If Mermaid styling limits make boxes and arrows feel too similar, reduce the number of visible group boundaries or move detail into multiple smaller diagrams.

## Applying In Draw.io

- Use bold box labels by default.
- Keep fill, stroke, and font colors explicit when readability depends on them.
- Use tinted containers for grouped sections instead of heavy decorative framing.
- Give container outlines a different stroke color, opacity, or weight than connectors.
- Do not let dashed container borders visually compete with dashed optional arrows; if both are needed, differentiate them by color or stroke weight.
- In complex Draw.io diagrams, use subtle fills and quieter boundaries for areas so connectors remain the strongest directional marks on the page.
