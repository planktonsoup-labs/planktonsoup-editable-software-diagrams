# Skill Operational Guidance

This repository provides reusable guidance for generating and maintaining editable software diagrams.

This file is the canonical, agent-neutral source of operational guidance for this skill.
Compatibility entrypoints (for example, `CLAUDE.md`, `.github/copilot-instructions.md`, and other adapter files) should reference this file rather than duplicating rules.

## Scope

This skill is focused on software-development diagrams in:

- Mermaid
- draw.io / diagrams.net

Do not broaden scope to other diagram systems unless explicitly requested.

## Source Of Truth

- `SKILL.md` defines skill contract and behavior expectations.
- `references/styling.md` defines cross-format styling guidance.
- `references/mermaid.md` defines Mermaid-specific guidance.
- `references/drawio.md` defines draw.io-specific guidance.
- `README.md` explains repository layout and usage.

## Required Behaviors

- Keep outputs editable, diff-friendly, and human-reviewable.
- Preserve source fidelity when editing existing diagrams.
- Prefer text-first diagram workflows when feasible.
- Keep Mermaid and draw.io rules clearly separated.
- Keep labels concise and reduce unnecessary visual clutter.

## Operating Workflow

1. Read this file and `SKILL.md`.
2. Select a relevant template, prompt, or example when available.
3. Generate or edit diagram source in the requested format.
4. Verify output is maintainable and source-control friendly.
5. Avoid image-only deliverables unless explicitly requested.

## Maintenance Rules

- Keep this file neutral across agent ecosystems.
- Avoid duplicating full instruction sets in adapter files.
- Use adapters as discovery and compatibility surfaces that delegate here.
- Keep changes small, coherent, and aligned with current repository structure.
