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

## Raw XML Requirements

Treat Form B raw-XML draw.io files as a strict schema.

- Save raw `.drawio` XML as UTF-8 without BOM.
- Use `compressed="false"` for raw XML.
- `<diagram>` must contain exactly one child element: `<mxGraphModel>`.
- Do not place comments anywhere inside `<diagram>`.
- Do not place stray text nodes, unknown tags, or whitespace-only content inside `<diagram>` or `<root>` beyond normal XML formatting.
- `<root>` must contain only `<mxCell>` elements.
- Keep the two base cells present:
  - `<mxCell id="0" />`
  - `<mxCell id="1" parent="0" />`
- After those base cells, keep only valid `<mxCell>` elements.
- Do not mix raw XML with encoded fragments, partial encoding, or base64 payloads.
- Avoid XML declarations for these raw files.

If draw.io shows an `atob` decoding error for a raw-XML file, treat that as a parser failure first, not as an encoding problem.

## Editing Existing Encoded Files

Projects may already contain `.drawio` files that are not stored as readable raw XML.

- If `compressed="true"`, the `<diagram>` content is compressed/encoded diagram payload, not directly editable raw XML.
- Some project files may also contain encoded diagram payloads that need to be decoded before safe source editing.
- For maintenance work, decode those files back to raw XML first, inspect and edit the real `mxGraphModel`, then decide whether to keep the project format or normalize to `compressed="false"`.
- Do not attempt structural edits against compressed payload text directly.

When editing an existing encoded draw.io file:

1. Detect whether the file is raw XML or encoded/compressed diagram content.
2. If encoded or compressed, decode it back to raw XML first.
3. Make structural edits against the decoded `mxGraphModel`.
4. Validate the raw XML structure and rendering.
5. Preserve the original storage format only when the project already depends on it or the user explicitly wants it preserved. Otherwise prefer `compressed="false"` for maintainability.

If you cannot decode the existing payload back to inspectable raw XML, do not guess at edits inside the encoded content.

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
- Do not insert XML comments inside `<diagram>`, `<mxGraphModel>`, or `<root>`.
- Do not place non-`mxCell` elements inside `<root>`.
- Do not leave stray text nodes inside `<diagram>` or `<root>`.
- Prefer connectors with explicit `source` and `target` IDs.
- After editing, preview the file and verify arrowheads visibly land on the intended shape edges.
- When a connector targets a broad container and renders poorly, connect it to a nearby concrete node or add a small anchor node to make the relationship unambiguous.
- Always set an explicit page background color for new diagrams so readability does not depend on dark-mode or light-mode editor defaults.
- Allow transparent labels only for non-connector text that sits on that known page background or inside a filled shape with sufficient contrast.
- When transparent labels are used, set `Background Style = None` so draw.io uses the simpler SVG/text fallback path instead of the theme-dependent HTML label layer.
- This is a source-level rendering rule, not just a styling preference: `Background Style = None` changes the label rendering path so transparent text resolves against the page instead of the editor canvas.
- When hand-editing XML for transparent non-connector labels, set `labelBackgroundColor=none;` when needed so the label reveals the page background or local filled surface instead of a theme-colored plate.
- When a label crosses connectors, mixed fills, or busy areas, do not rely on transparency alone.
- In those cases, keep the label near the connector segment or branch it explains and use a dedicated label vertex with a light neutral fill at roughly `70%` `fillOpacity` so the page and nearby constructs still show through instead of being fully blocked.
- For edge labels, branch labels such as `Yes` and `No`, and other connector-adjacent annotations, that dedicated semi-opaque label-vertex treatment is required even when the underlying page is otherwise readable.
- Do not assume `labelBackgroundColor` with a nominal background-opacity setting will render translucently for connector labels. In practice, use a separate label vertex with explicit `fillOpacity=70`, keep `opacity=100` and `textOpacity=100`, require `Background Style = None`, and remove the border unless it carries meaning.
- In the dark diagrams.net editor, under-specified labels can fall back to black plates, black rectangles, or canvas-punch-through transparency. Treat that as a bug and correct it by setting the page background color first, then using `Background Style = None`, then adjusting label styling only if needed.
- For edge labels, use one of these patterns:
  - A separate small label vertex near the connector, with `fillOpacity=70`, readable text color, `Background Style = None`, and little or no visible border
  - Inline edge label rendering only when preview verification confirms the label background is actually translucent and not merely light-colored or fully opaque
- For branch labels such as `Yes`, `No`, success/failure, or protocol annotations, keep the label anchored at the branch it describes and add the local semi-opaque background there instead of moving the label to a quieter but less meaningful location.
- In dense diagrams, use the separate label vertex pattern unless a simpler inline label demonstrably passes all required render checks.

## XML Safety Checklist

Before considering a raw-XML `.drawio` file complete, verify all of the following:

- File encoding is UTF-8 without BOM
- No comments anywhere inside `<diagram>`
- `<diagram>` contains only `<mxGraphModel>`
- `<root>` contains only `<mxCell>`
- All HTML in `value=` attributes is escaped
- No stray text nodes or unknown tags
- Every `id` is unique
- Every `parent` points to an existing cell
- Every edge `source` and `target` points to an existing cell
- Every vertex has an `mxGeometry` child with `as="geometry"`

If any of these fail, fix the XML structure before troubleshooting rendering.

## Local CLI Validation

- Check for a local `drawio`, `draw.io`, or `diagrams.net` executable before relying on manual inspection alone.
- Probe cross-platform in this order: `PATH`, then common package-manager shim directories, then common app install locations for the current OS.
- On Windows, common fallback locations include Scoop shims, Chocolatey `bin`, `%AppData%\\npm` wrappers when used, and explicit desktop-app install folders.
- On macOS, common fallback locations include `/opt/homebrew/bin`, `/usr/local/bin`, `/Applications/draw.io.app`, `/Applications/diagrams.net.app`, and `~/Applications`.
- On Linux, common fallback locations include `~/.local/bin`, `/usr/local/bin`, `/usr/bin`, `/snap/bin`, Flatpak exports, and AppImage-style install paths.
- If one is available, inspect `--help` when needed and use the narrowest local export or render-oriented command that validates the edited `.drawio` file.
- Prefer validating the exact final file rather than a reconstructed copy.
- If no local draw.io CLI is installed, fall back to diagrams.net preview checks and manual XML review of changed connectors, labels, and page settings, and note that CLI validation was unavailable.
