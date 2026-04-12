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
- Allow transparent labels only for non-connector text that sits on that known page background or inside a filled shape with sufficient contrast.
- When transparent labels are used, set `Background Style = None` so draw.io uses the simpler SVG/text fallback path instead of the theme-dependent HTML label layer.
- This is a source-level rendering rule, not just a styling preference: `Background Style = None` changes the label rendering path so transparent text resolves against the page instead of the editor canvas.
- When hand-editing XML for transparent non-connector labels, set `labelBackgroundColor=none;` when needed so the label reveals the page background or local filled surface instead of a theme-colored plate.
- When a label crosses connectors, mixed fills, or busy areas, do not rely on transparency alone.
- In those cases, keep the label near the connector segment or branch it explains and use a dedicated label vertex with a light neutral fill at roughly `50%` `fillOpacity` so the page and nearby constructs still show through instead of being fully blocked.
- For edge labels, branch labels such as `Yes` and `No`, and other connector-adjacent annotations, that dedicated semi-opaque label-vertex treatment is required even when the underlying page is otherwise readable.
- Do not assume `labelBackgroundColor` with a nominal background-opacity setting will render translucently for connector labels. In practice, use a separate label vertex with explicit `fillOpacity=50`, keep `opacity=100` and `textOpacity=100`, require `Background Style = None`, and remove the border unless it carries meaning.
- In the dark diagrams.net editor, under-specified labels can fall back to black plates, black rectangles, or canvas-punch-through transparency. Treat that as a bug and correct it by setting the page background color first, then using `Background Style = None`, then adjusting label styling only if needed.
- For edge labels, prefer one of these patterns:
-  - A separate small label vertex near the connector, with `fillOpacity=50`, readable text color, `Background Style = None`, and little or no visible border
-  - Inline edge label rendering only when preview verification confirms the label background is actually translucent and not merely light-colored or fully opaque
- For branch labels such as `Yes`, `No`, success/failure, or protocol annotations, keep the label anchored at the branch it describes and add the local semi-opaque background there instead of moving the label to a quieter but less meaningful location.
- In dense diagrams, prefer the separate label vertex pattern, but do not add fully opaque label boxes everywhere when a stable canvas background preserves readability with less visual clutter.

## Local CLI Validation

- Check for a local `drawio`, `draw.io`, or `diagrams.net` executable before relying on manual inspection alone.
- Probe cross-platform in this order: `PATH`, then common package-manager shim directories, then common app install locations for the current OS.
- On Windows, common fallback locations include Scoop shims, Chocolatey `bin`, `%AppData%\\npm` wrappers when used, and explicit desktop-app install folders.
- On macOS, common fallback locations include `/opt/homebrew/bin`, `/usr/local/bin`, `/Applications/draw.io.app`, `/Applications/diagrams.net.app`, and `~/Applications`.
- On Linux, common fallback locations include `~/.local/bin`, `/usr/local/bin`, `/usr/bin`, `/snap/bin`, Flatpak exports, and AppImage-style install paths.
- If one is available, inspect `--help` when needed and use the narrowest local export or render-oriented command that validates the edited `.drawio` file.
- Prefer validating the exact final file rather than a reconstructed copy.
- If no local draw.io CLI is installed, fall back to diagrams.net preview checks and manual XML review of changed connectors, labels, and page settings, and note that CLI validation was unavailable.
