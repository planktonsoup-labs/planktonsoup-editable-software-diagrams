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
- All text must visibly contrast with the immediate surface behind it.
- Do not use white or near-white text on light fills, light page areas, or light local label treatments.
- Do not use dark or near-dark text on dark fills, dark page areas, or dark local label treatments.
- If text sits on a mixed, busy, or ambiguous surface, change the text color or local treatment until the text is distinctly readable.
- Treat weak text contrast as a correctness failure, not as optional polish.

## Connectors

- Use visible arrowheads and at least medium-weight connector strokes.
- Use solid connectors for normal required interactions.
- Use dashed connectors or dashed borders for optional, replaceable, or extensible parts.
- Keep connector color and stroke treatment distinct from container or section outlines.
- In dense diagrams, prefer darker or more directional arrow styling and quieter container borders so flows remain legible.
- If dense diagrams still look like a field of similar lines, simplify the structure or split the diagram instead of trying to solve it with more lines.
- Do not let separate connectors ride the same visual path for long stretches when that makes them read as one line.

## Layout

- Keep diagrams compact enough for common doc widths.
- Favor one dominant flow direction.
- Leave enough whitespace that later edits do not force immediate re-layout.
- Add a legend when more than three semantic colors are in play.
- Offset, reroute, or shorten lines that would otherwise overlap or stack on the same trajectory.
