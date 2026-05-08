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

- `SKILL.md` is the runtime skill contract consumed when agents use this skill.
- `references/styling.md` defines cross-format styling guidance.
- `references/mermaid.md` defines Mermaid-specific guidance.
- `references/drawio.md` defines draw.io-specific guidance.
- `README.md` explains repository layout and usage.

## File Boundary

- `AGENTS.md` is for maintainers and agent authors who evolve, govern, and maintain this skill repository.
- `SKILL.md` is the skill itself: the execution contract that agents consume at runtime when the skill is used.
- `README.md` is onboarding and adoption documentation: what the skill does, how to install/use it, and where key resources live.
- Do not move maintainer-only governance content into `SKILL.md`.
- Do not move runtime behavior rules out of `SKILL.md` into `AGENTS.md`.
- Do not treat `README.md` as the canonical source for runtime behavior or repository governance.
- Keep both files aligned but purpose-separated: `AGENTS.md` governs maintenance, `SKILL.md` governs runtime behavior.

## README Boundary

- `README.md` should explain outcomes, installation, compatibility, examples, and repository navigation.
- `README.md` should not duplicate full operational policy from `AGENTS.md`.
- `README.md` should not duplicate full runtime contract details from `SKILL.md`.
- When policy detail is needed, `README.md` should link to `AGENTS.md` or `SKILL.md` instead of copying large rule blocks.

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

## README Maintenance Rules

- Treat `README.md` as onboarding and discoverability documentation for humans and agents.
- Keep `README.md` outcome-oriented: show what the repository produces and how to use it quickly.
- Prefer concise, embeddable examples (including Mermaid diagrams) over long theory sections.
- Keep "skill installation" guidance focused on installing and using this skill, not on adapter authoring internals.
- When documenting default file locations or fallback paths for agent ecosystems, verify against official docs first.
- If an ecosystem does not document a stable filesystem location, say so explicitly instead of guessing.
- Keep operational behavior details canonical in `AGENTS.md` and `SKILL.md`; `README.md` should reference, not duplicate, full operational rules.
