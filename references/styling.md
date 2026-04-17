# Diagram Styling Reference

Use when repo does not already provide stronger diagram convention.

## Goals

- Maximize readability in rendered diagrams and raw source review
- Preserve semantic meaning through consistent color use
- Keep diagrams maintainable by humans and agents
- Make containment and flow visually distinct at a glance, especially in dense technical diagrams

## Default Palette Strategy

Dark high-contrast node fills, white text. Small set of semantic color families used consistently:

- green — user-owned logic, application code, product-specific components
- blue — framework, platform, infrastructure components
- orange — adapters, boundaries, configuration-oriented pieces
- red — provider-specific plugins, warnings, security-sensitive elements
- purple — runtime services, cross-cutting concerns
- blue-grey/slate — neutral labels, legends, non-primary supporting elements

Containers/background sections: lighter tints of same semantic family, darker outline.
No styling containers and connectors as same kind of line. Grouping boundaries = structural framing, not another arrow.
Treat this distinction as priority, not optional polish.

## Labeling

- Clear diagram title.
- Short subtitle, caption, or nearby context sentence explaining scope.
- Bold labels for major nodes/groups.
- Regular text for small annotations, examples, code-like snippets.
- All text must visibly contrast with immediate surface behind it.
- No white/near-white text on light fills, light page areas, or light local label treatments.
- No dark/near-dark text on dark fills, dark page areas, or dark local label treatments.
- Text on mixed/busy/ambiguous surface → change text color or local treatment until distinctly readable.
- Weak text contrast = correctness failure, not optional polish.

## Connectors

- Visible arrowheads, at least medium-weight connector strokes.
- Solid connectors for normal required interactions.
- Dashed connectors/borders for optional, replaceable, or extensible parts.
- Connector color/stroke treatment distinct from container or section outlines.
- Dense diagrams: darker/more directional arrow styling, quieter container borders so flows stay legible.
- Dense diagram looks like field of similar lines → simplify structure or split diagram.
- No separate connectors riding same visual path for long stretches.

## Layout

- Compact enough for common doc widths.
- One dominant flow direction.
- Enough whitespace that later edits don't force immediate re-layout.
- Legend when >3 semantic colors in play.
- Offset, reroute, or shorten lines that would otherwise overlap or stack.
