# draw.io Reference

Use when diagram needs canvas-style editing, manual positioning, or richer mixed layouts than Mermaid handles.

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

Always set page bg color explicitly. No assume light editor canvas = white page in dark mode.

## Raw XML Requirements

Treat raw-XML draw.io files as strict schema.

- UTF-8 no BOM.
- `compressed="false"` for raw XML.
- `<diagram>` must have exactly one child: `<mxGraphModel>`.
- No comments inside `<diagram>`.
- No stray text nodes, unknown tags, whitespace-only content inside `<diagram>` or `<root>` beyond normal XML formatting.
- `<root>` contains only `<mxCell>` elements.
- Keep two base cells:
  - `<mxCell id="0" />`
  - `<mxCell id="1" parent="0" />`
- After base cells: only valid `<mxCell>` elements.
- No mixing raw XML with encoded fragments, partial encoding, or base64 payloads.
- No XML declarations for raw files.

draw.io shows `atob` decoding error for raw-XML file → parser failure first, not encoding problem.

## Editing Existing Encoded Files

Projects may have `.drawio` files not stored as readable raw XML.

- `compressed="true"` → `<diagram>` content = compressed/encoded payload, not directly editable.
- Some files need decode before safe source edit.
- Maintenance: decode to raw XML first, inspect/edit real `mxGraphModel`, then decide keep format or normalize to `compressed="false"`.
- No structural edits against compressed payload text directly.

When editing existing encoded file:

1. Detect: raw XML or encoded/compressed?
2. Encoded/compressed → decode to raw XML first.
3. Structural edits against decoded `mxGraphModel`.
4. Validate raw XML structure + rendering.
5. Preserve original format only when project depends on it or user explicitly wants. Otherwise prefer `compressed="false"`.

Cannot decode payload → no guess edits inside encoded content.

## Common Shape Pattern

Each visible shape = `mxCell` with:

- `vertex="1"` for node
- `edge="1"` for connector
- `parent="1"` on normal page content
- `mxGeometry` child

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

Prefer explicit `source`/`target` IDs — survive later layout edits.

## Layout Heuristics

- Primary flow left-to-right or top-to-bottom consistently.
- Align sibling nodes to common x or y coords.
- Leave whitespace for later edits.
- Containers sparingly; group only when grouping conveys meaning.
- Use containers/section bgs for environments, trust zones, bounded contexts, infra layers when distinctions matter.

## Editing Rules

- Keep IDs stable in existing files.
- Add only cells needed for req change.
- Readable numeric coords on loose grid (10 or 20px).
- Prefer simple built-in styles: `rounded=1`, `whiteSpace=wrap`, `html=1`, `endArrow=block`.
- No compressed payloads for new files unless compat requires.
- One page unless user asks more.
- Preserve existing page ID + root structure when editing.
- Hand-editable XML over tool-generated churn.
- No XML comments inside `<diagram>`, `<mxGraphModel>`, or `<root>`.
- No non-`mxCell` elements inside `<root>`.
- No stray text nodes inside `<diagram>` or `<root>`.
- Prefer connectors with explicit `source`/`target` IDs.
- After edit: preview, verify arrowheads visibly land on intended shape edges.
- After moving containers/groups: verify routed connectors still attach to intended targets.
- Connector targets broad container + renders poorly → connect to nearby concrete node or add small anchor node.
- Explicit connector stroke/arrow colors contrasting with page bg. No white/near-white connector styling on light pages.
- Always set explicit page bg color for new diagrams. No rely on dark/light editor defaults.
- All draw.io text contrasts with local surface: node text, container titles, notes, legends, connector labels.
- No white/near-white text on light areas/fills/labels. No dark text on dark fills.
- Transparent labels: only for non-connector text on known page bg or inside filled shape with sufficient contrast.
- Transparent labels → set `Background Style = None` → draw.io uses SVG/text fallback instead of theme-dependent HTML label layer.
- Source-level rendering rule: `Background Style = None` changes label rendering path → transparent text resolves against page not editor canvas.
- Hand-editing XML for transparent non-connector labels → set `labelBackgroundColor=none;` → label reveals page bg instead of theme-colored plate.
- Label crosses connectors/mixed fills/busy areas → no rely on transparency alone.
- Those cases: keep label near connector segment/branch it explains, use dedicated label vertex with light neutral fill ~70% `fillOpacity` so page/nearby constructs still show through.
- Edge labels, branch labels (`Yes`/`No`), connector-adjacent annotations → dedicated semi-opaque label-vertex treatment **required** even when underlying page otherwise readable.
- Connector-label text color must contrast with label fill. No white/near-white text on light neutral label fills.
- No assume `labelBackgroundColor` with nominal opacity renders translucently for connector labels. Use separate label vertex with explicit `fillOpacity=70`, `opacity=100`, `textOpacity=100`, `Background Style = None`. Remove border unless it carries meaning.
- Dark diagrams.net editor: under-specified labels → black plates, black rects, canvas-punch-through. Fix: set page bg first → `Background Style = None` → adjust label styling only if needed.
- Edge label patterns (pick one):
  - Separate small label vertex near connector: `fillOpacity=70`, readable text color, `Background Style = None`, little/no border
  - Inline edge label only when preview confirms label bg is actually translucent (not just light-colored or opaque)
- Branch labels (`Yes`/`No`, success/failure, protocol annotations): anchored at branch, add local semi-opaque bg there instead of moving to quieter location.
- Dense diagrams → use separate label vertex pattern unless simpler inline label passes all required render checks.

## XML Safety Checklist

Before raw-XML `.drawio` file complete:

- UTF-8 no BOM
- No comments inside `<diagram>`
- `<diagram>` contains only `<mxGraphModel>`
- `<root>` contains only `<mxCell>`
- Connector strokes/arrowheads visibly contrast with page bg
- All text visibly contrasts with immediate page, fill, or local label surface
- Connector-label text contrasts with label fill + page bg
- All HTML in `value=` attributes escaped
- No stray text nodes or unknown tags
- Every `id` unique
- Every `parent` points to existing cell
- Every edge `source`/`target` points to existing cell
- Every vertex has `mxGeometry` child with `as="geometry"`

Any fail → fix XML structure before troubleshooting rendering.

## Local CLI Validation

- Check for local `drawio`, `draw.io`, or `diagrams.net` executable before relying on manual inspection.
- Probe cross-platform: `PATH` → pkg-manager shims → OS app install locations.
- Windows fallbacks: Scoop shims, Chocolatey `bin`, `%AppData%\\npm` wrappers, explicit desktop-app install folders.
- macOS fallbacks: `/opt/homebrew/bin`, `/usr/local/bin`, `/Applications/draw.io.app`, `/Applications/diagrams.net.app`, `~/Applications`.
- Linux fallbacks: `~/.local/bin`, `/usr/local/bin`, `/usr/bin`, `/snap/bin`, Flatpak exports, AppImage paths.
- Available → inspect `--help` when needed; use narrowest local export/render cmd that validates the `.drawio` file.
- Validate exact final file, not reconstructed copy.
- No local draw.io CLI → fallback to diagrams.net preview + manual XML review of changed connectors, labels, page settings; note CLI unavailable.
