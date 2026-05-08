# [Diagram Title]

Briefly explain what this diagram shows, which subsystem or workflow it covers, and what level of abstraction it uses.

## Diagram

```mermaid
flowchart TD
    client[Client]
    api[API]
    db[(Database)]

    client --> api
    api --> db
```

## Notes

- Keep terminology aligned with the codebase.
- Keep Mermaid labels parser-safe. Prefer plain phrases over embedded quoted code examples.
- Do not use literal `\n` in Mermaid labels; use `<br/>` for visual line breaks when supported.
- Call out notable boundaries, assumptions, or optional flows here.
- Link to related diagrams or design notes when useful.
