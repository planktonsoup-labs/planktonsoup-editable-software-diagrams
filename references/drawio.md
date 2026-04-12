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
