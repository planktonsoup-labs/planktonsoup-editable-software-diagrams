# planktonsoup-editable-software-diagrams

A GitHub Copilot/agent skill for creating and updating software diagrams in human-readable, source-friendly formats.

## What this repo is

This repository defines the `planktonsoup-editable-software-diagrams` agent skill. It is optimized for:

- [Mermaid](https://mermaid.ai/) diagrams (`.mmd` or Markdown fenced code blocks)
- [draw.io](https://www.drawio.com/) diagrams (`.drawio`)

The skill is intended for architecture, flow, state, ERD, deployment, and integration diagrams that are easy for both humans and agents to review, edit, and version.

## Repository structure

- `SKILL.md` — Skill metadata, scope, and authoring guidance.
- `agents/openai.yaml` — Agent runtime configuration.
- `assets/blank.drawio` — Starter template for new [draw.io](https://www.drawio.com/) diagrams.
- `references/` — Style and format guidance for [Mermaid](https://mermaid.ai/) and [draw.io](https://www.drawio.com/).

## Usage

Use this skill when you want the agent to generate or modify editable diagrams rather than image-only outputs. Prefer [Mermaid](https://mermaid.ai/) for text-first diagrams and [draw.io](https://www.drawio.com/) for layout-heavy or canvas-oriented diagrams.

## Notes

- Preserve existing file formats when editing diagrams.
- Keep outputs diff-friendly and readable.
- Use the provided references to follow repository styling and syntax conventions.
