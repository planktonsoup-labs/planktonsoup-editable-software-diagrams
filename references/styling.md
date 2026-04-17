# Diagram Styling Reference

Use when repo provides no stronger diagram convention.

## Goals

- Max readability in rendered diagrams + raw source review
- Semantic meaning preserved through consistent color use
- Diagrams maintainable by humans + agents
- Containment/flow visually distinct at a glance — especially in dense technical diagrams

## Default Palette Strategy

Dark high-contrast node fills, white text. Small set of semantic color families used consistently:

- green — user-owned logic, app code, product-specific components
- blue — framework, platform, infra components
- orange — adapters, boundaries, config-oriented pieces
- red — provider-specific plugins, warnings, security-sensitive elements
- purple — runtime services, cross-cutting concerns
- blue-grey/slate — neutral labels, legends, non-primary supporting elements

Containers/bg sections: lighter tints of same semantic family, darker outline.
No styling containers + connectors as same kind of line. Grouping boundaries = structural framing, not another arrow.
Treat as priority, not optional polish.

## Labeling

- Clear diagram title.
- Short subtitle, caption, or nearby ctx sentence explaining scope.
- Bold labels for major nodes/groups.
- Regular text for small annotations, examples, code-like snippets.
- All text visibly contrasts with immediate surface behind it.
- No white/near-white text on light fills, light page areas, light local label treatments.
- No dark/near-dark text on dark fills, dark page areas, dark local label treatments.
- Text on mixed/busy/ambiguous surface → change text color or local treatment until distinctly readable.
- Weak text contrast = correctness failure, not optional polish.

## Connectors

- Visible arrowheads, at least medium-weight strokes.
- Solid connectors — normal required interactions.
- Dashed connectors/borders — optional, replaceable, or extensible parts.
- Connector color/stroke treatment distinct from container/section outlines.
- Dense diagrams: darker/more directional arrow styling, quieter container borders so flows stay legible.
- Dense diagram looks like field of similar lines → simplify structure or split diagram.
- No separate connectors riding same visual path for long stretches.

## Layout

- Compact enough for common doc widths.
- One dominant flow direction.
- Enough whitespace that later edits don't force immediate re-layout.
- Legend when >3 semantic colors in play.
- Offset, reroute, or shorten lines that would otherwise overlap or stack.
