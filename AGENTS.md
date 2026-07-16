# AGENTS.md

Guidance for agents working on digital-person, a repository of portable person definitions, reference material, skills, and templates for agent systems.

## Repository model

- `template/` is the reusable starting point; keep it generic and free of a real person's private data.
- `person/` is the concrete deployment/content layer.
- `skills/` contains procedural capabilities with their own entry instructions.
- `reference/` contains supporting knowledge rather than executable instructions.

## Rules

- Preserve the distinction between template and deployment. A reusable improvement belongs in `template/`; personal facts do not.
- Treat identity, biography, preferences, contacts, and private source material as sensitive. Do not infer, invent, or expose facts.
- Keep instruction precedence explicit and avoid contradictory duplicate guidance across files.
- Skill entry files must state triggers, inputs, outputs, limitations, and verification. Follow referenced paths relative to the skill.
- Prefer small Markdown files with durable language over tool-specific generated state.

## Validation

Check all relative links and referenced paths, search for placeholder leakage and secrets, and review the rendered Markdown hierarchy. When changing templates, instantiate a disposable example mentally or in a temporary directory to ensure the instructions are complete without relying on this deployment. Keep commits scoped to one conceptual change.
