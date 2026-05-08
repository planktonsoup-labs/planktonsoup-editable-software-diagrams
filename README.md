# planktonsoup-editable-software-diagrams

An agent skill for creating and updating software-development diagrams in human-readable, source-friendly formats.

## Overview

This directory defines the `planktonsoup-editable-software-diagrams` agent skill. It is optimized for:

- [Mermaid][mermaid] diagrams (`.mmd` or Markdown fenced code blocks)
- [draw.io][drawio] diagrams (`.drawio`)

The skill is intended for architecture, flow, state, ERD, deployment, and integration diagrams that are easy for both humans and agents to review, edit, and version.

## Structure

- `SKILL.md` — Skill metadata, scope, and authoring guidance.
- `AGENTS.md` — Canonical operational documentation for this skill.
- `CLAUDE.md` — Claude adapter that delegates to canonical guidance.
- `.github/copilot-instructions.md` — Copilot adapter that delegates to canonical guidance.
- `.cursor/rules/diagrams.mdc` — Cursor adapter rules.
- `adapters/` — Additional compatibility entrypoints for agent tooling.
- `agents/openai.yaml` — Runtime metadata for environments that use this file convention.
- `assets/blank.drawio` — Starter template for new [draw.io][drawio] diagrams.
- `assets/mermaid-doc-template.md` — Starter template for Mermaid diagrams hosted in Markdown.
- `examples/invocations/` — Invocation examples for different agent tools.
- `prompts/` — Prompt starters for diagram workflows.
- `templates/` — Reusable template files for diagram work.
- `references/styling.md` — General visual guidance shared across diagram formats.
- `references/mermaid.md` — Mermaid-specific syntax, validation, and editing guidance.
- `references/drawio.md` — draw.io-specific XML, rendering, and validation guidance.

## Usage

Use this skill when you want the agent to generate or modify editable diagrams rather than image-only outputs. Prefer [Mermaid][mermaid] for text-first diagrams and [draw.io][drawio] for layout-heavy or canvas-oriented diagrams.

## Notes

- Preserve existing file formats when editing diagrams.
- Keep outputs diff-friendly and readable.
- Use the provided references to follow repository styling and syntax conventions.

## For Agent Tools

This repository includes compatibility adapters for:

- Claude Code (`CLAUDE.md`)
- GitHub Copilot (`.github/copilot-instructions.md`)
- Cursor (`.cursor/rules/`)
- Codex (`adapters/codex.md`)

Canonical guidance remains in:

- `AGENTS.md`

[mermaid]: https://mermaid.ai/
[drawio]: https://www.drawio.com/
