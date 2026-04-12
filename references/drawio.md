# draw.io Reference

Use draw.io when a software-development diagram needs canvas-style editing, manual positioning, or richer mixed layouts than Mermaid handles comfortably.

## Minimal File Structure

Prefer uncompressed XML:

```xml
<mxfile host="app.diagrams.net" compressed="false">
  <diagram name="Page-1" id="page-1">
    <mxGraphModel dx="1200" dy="800" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1100" pageHeight="850" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

Always set the page background color explicitly for new diagrams. Do not assume a light-looking editor canvas behaves like a real white page in dark mode.

## Common Shape Pattern

Each visible shape is usually an `mxCell` with:

- `vertex="1"` for a node
- `edge="1"` for a connector
- `parent="1"` on normal page content
- An `mxGeometry` child

Typical rectangle node:

```xml
<mxCell id="api" value="API" style="rounded=1;whiteSpace=wrap;html=1;" vertex="1" parent="1">
  <mxGeometry x="320" y="140" width="140" height="60" as="geometry" />
</mxCell>
```

Typical connector:

```xml
<mxCell id="edge-client-api" style="endArrow=block;html=1;rounded=0;" edge="1" parent="1" source="client" target="api">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

Prefer this pattern over freehand line placement because explicit `source` and `target` IDs survive later layout edits more reliably.

## Layout Heuristics

- Place primary flow left-to-right or top-to-bottom consistently.
- Align sibling nodes to common x or y coordinates.
- Leave enough whitespace for new edits later.
- Use containers sparingly; only group when the grouping conveys meaning.
- Use containers and section backgrounds to show environments, trust zones, bounded contexts, or infrastructure layers when those distinctions matter.

## Editing Rules

- Keep IDs stable if the file already exists.
- Add only the cells needed for the requested change.
- Use readable numeric coordinates on a loose grid such as 10 or 20 pixels.
- Prefer simple built-in styles like `rounded=1`, `whiteSpace=wrap`, `html=1`, `endArrow=block`.
- Avoid compressed payloads for new files unless compatibility requires them.
- Prefer one page unless the user explicitly asks for more.
- Preserve the existing page ID and root structure when editing an existing file.
- Prefer hand-editable XML over tool-generated churn.
- Prefer connectors with explicit `source` and `target` IDs.
- After editing, preview the file and verify arrowheads visibly land on the intended shape edges.
- When a connector targets a broad container and renders poorly, connect it to a nearby concrete node or add a small anchor node to make the relationship unambiguous.
- Always set an explicit page background color for new diagrams so readability does not depend on dark-mode or light-mode editor defaults.
- Allow transparent labels when they sit on that known page background or inside a filled shape with sufficient contrast.
- When transparent labels are used, set `Background Style = None` so draw.io uses the simpler SVG/text fallback path instead of the theme-dependent HTML label layer.
- This is a source-level rendering rule, not just a styling preference: `Background Style = None` changes the label rendering path so transparent text resolves against the page instead of the editor canvas.
- When hand-editing XML for transparent edge labels, also set `labelBackgroundColor=none;` on the edge style so the label reveals the page background or local filled surface instead of a theme-colored plate.
- When a label crosses connectors, mixed fills, or busy areas, use explicit label styling or a dedicated label box instead of relying on transparency alone.
- In the dark diagrams.net editor, under-specified labels can fall back to black plates, black rectangles, or canvas-punch-through transparency. Treat that as a bug and correct it by setting the page background color first, then using `Background Style = None`, then adjusting label styling only if needed.
- For edge labels, prefer one of these patterns:
  - Transparent or minimally styled text on a stable readable page background or filled local surface, with `Background Style = None` and `labelBackgroundColor=none;` in source-edited XML
  - Explicit edge label styling, including background only when needed, when the text crosses lines or busy areas
  - A separate small label vertex near the connector, with its own fill and stroke, when precise placement or theme-safe readability matters more than compact XML
- Prefer the separate label vertex pattern in dense diagrams, but do not add opaque label boxes everywhere when a stable canvas background preserves readability with less visual clutter.
