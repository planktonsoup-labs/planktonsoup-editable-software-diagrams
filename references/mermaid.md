# Mermaid Reference

Use Mermaid when diagram expressible as structured text without precise manual positioning.

## Good Fits

- `flowchart` — workflows, request paths, branching logic, service maps, architecture overviews
- `sequenceDiagram` — interactions over time between users, apps, services, workers, providers
- `stateDiagram-v2` — lifecycle transitions in entities, jobs, runtime components
- `classDiagram` — type/module relationships when those relationships matter to design
- `erDiagram` — data models, storage relationships
- `journey`, `timeline`, `gantt`, `pie`, `gitGraph`, `mindmap` — when request clearly matches

## Choose Diagram Type Fast

- `flowchart` for process/architecture overviews.
- `sequenceDiagram` when order and message timing matter.
- `stateDiagram-v2` for status transitions, lifecycle rules.
- `erDiagram` for tables, entities, cardinality.
- `classDiagram` only when type relationships are the point.
- Prefer `flowchart` over `classDiagram` for service architecture unless code structure is real subject.

## Practical Patterns

### Flowchart

```mermaid
flowchart TD
    client[Client] --> api[API]
    api --> db[(Database)]
    api --> queue[Queue]
```

### Sequence Diagram

```mermaid
sequenceDiagram
    participant U as User
    participant W as Web App
    participant A as API
    U->>W: Submit form
    W->>A: POST /orders
    A-->>W: 201 Created
    W-->>U: Confirmation
```

### ER Diagram

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    ORDER ||--|{ ORDER_ITEM : contains
```

## Editing Rules

- One diagram per fenced block/file unless user explicitly wants multiple.
- Simple labels over inline paragraphs.
- Subgraphs only when they materially clarify grouping.
- No over-styling with classes unless styling carries meaning.
- Markdown has surrounding headings/prose → edit only relevant fenced block.
- Save Mermaid source and hosting Markdown as UTF-8 without BOM.
- Preserve existing node IDs unless actively harmful.
- Verify every referenced node ID declared exactly once; edges render to intended node after rename/refactor.
- Unsupported syntax → pick simpler Mermaid construct.
- All Mermaid text must contrast with rendered surface behind it: node labels, section labels, notes, connector labels.
- No Mermaid theme/class styling that blends text into page, node fill, or label surface.
- Label text = part of syntax validation. Use plain language. No embedded double quotes, escaped quotes, dense punctuation.
- No `\n` as portable line-break. Use Mermaid-supported break (`<br/>` where supported) or shorten/split wording.
- Connector labels: visually plain, background-transparent. No draw.io-style backfills, filled label plates, semi-opaque boxes.
- `mmdc` installed → use as default validation path, require successful render before edit complete.
- Preview when possible. Visually detached/ambiguous edges = correctness bugs.
- Long edge misses target → re-layout nodes, add intermediate anchor node, or split diagram.
- Prefer nearby concrete or anchor nodes over distant container-like targets.

## Local CLI Validation

- Check `mmdc` first.
- Probe cross-platform: `PATH` → package-manager shims → OS app install locations.
- Windows fallbacks: `%AppData%\\npm`, Scoop shims, Chocolatey `bin`, explicit install folders.
- macOS fallbacks: `/opt/homebrew/bin`, `/usr/local/bin`, `~/Applications`, `/Applications`.
- Linux fallbacks: `~/.local/bin`, `/usr/local/bin`, `/usr/bin`, `/snap/bin`, AppImage paths.
- Standalone `.mmd` → render directly.
- Mermaid in Markdown → extract fenced block to temp `.mmd`, render that file.
- `mmdc` not installed → fall back to Mermaid-compatible preview or manual structural validation; note CLI unavailable. Do not skip validation.

## Label Safety

- Prefer `Add apple`, `Cache hit`, `POST /orders` over embedded source-code examples.
- Exact code/string literals like `Add("apple")` → put in surrounding Markdown, not inside label.
- Need visual line break → `<br/>` if family supports it; else shorten to single-line phrase, move detail to prose.
- No escaped quotes (`\"apple\"`) in labels. Some renderers reject even when structure valid.
- No literal `\n` in labels unless syntax explicitly interprets it as line break and preview confirms.
- Parser error near label → simplify label first, re-check before broader changes.

### Safer Example

Instead of:

```mermaid
flowchart TD
    list -->|"Add(\"apple\"), Add(\"banana\")"| state
```

Prefer:

```mermaid
flowchart TD
    list -->|"Add apple, Add banana"| state
```

Describe exact method calls in prose if precision matters.

## Hosting In Markdown

Host in Markdown when devs read diagram alongside technical explanation.

- Short heading immediately above block.
- 1–2 sentences of scope/interpretation above or below.
- Block self-contained — readable when previewed alone.
- Standalone `.mmd` → link from nearest `README.md` or design note if discoverability matters.
