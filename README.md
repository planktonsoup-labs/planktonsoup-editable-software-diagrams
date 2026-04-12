# editable-software-diagrams

A GitHub Copilot/agent skill for creating and updating software diagrams in human-readable, source-friendly formats.

## What this repo is

This repository defines the `editable-software-diagrams` agent skill. It is optimized for:

- Mermaid diagrams (`.mmd` or Markdown fenced code blocks)
- draw.io diagrams (`.drawio`)

The skill is intended for architecture, flow, state, ERD, deployment, and integration diagrams that are easy for both humans and agents to review, edit, and version.

## Repository structure

- `SKILL.md` — Skill metadata, scope, and authoring guidance.
- `agents/openai.yaml` — Agent runtime configuration.
- `assets/blank.drawio` — Starter template for new draw.io diagrams.
- `references/` — Style and format guidance for Mermaid and draw.io.

## Usage

Use this skill when you want the agent to generate or modify editable diagrams rather than image-only outputs. Prefer Mermaid for text-first diagrams and draw.io for layout-heavy or canvas-oriented diagrams.

## Notes

- Preserve existing file formats when editing diagrams.
- Keep outputs diff-friendly and readable.
- Use the provided references to follow repository styling and syntax conventions.
