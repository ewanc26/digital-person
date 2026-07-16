# AGENTS.md

Guidance for agents working on this Markdown-only Letta “digital person” memory repository.

## Content model

- `template/` is the generic four-file skeleton: `persona.md`, `online-presence.md`, `voice.md`, and `human/identity.md`.
- `person/faol/` is a filled deployment of that same structure. It contains concrete identity, account, relationship, preference, and behavioural material; treat it as personal data even though it is tracked.
- `skills/` contains prose playbooks for Bluesky, blogging, email, Telegram, and web browsing. These files describe behaviour and example API calls but provide no executable integration, dependency manifest, or test harness.
- `reference/platform-conventions.md` is shared etiquette/context, not a higher-precedence instruction file.

## Editing rules

- A reusable structural or behavioural improvement belongs in `template/`; a fact or preference unique to one deployment belongs under `person/<name>/`. Decide deliberately whether a template change also needs a reviewed deployment update—do not overwrite filled content mechanically.
- Never invent identity details, opinions, relationships, access, or consent. Avoid expanding tracked personal/contact information and do not place account tokens, bot tokens, email credentials, private correspondence, or session data here.
- The deployed files intentionally diverge from the templates in places. Preserve the deployed voice and boundaries unless the requested change explicitly updates that person.
- Several skills link to `[[template/...]]`, even when applied to a deployed person. Resolve those repository-root wiki links consciously and prefer the active person's corresponding file when the runtime has selected one.
- Four skill files use YAML front matter with `name` and `description`; `skills/telegram/SKILL.md` currently does not. Preserve loader compatibility when normalizing metadata and do not assume every agent runtime understands wiki links or this skill format.
- Platform limits, APIs, social norms, and named tools such as `web_search`/`fetch_webpage` can drift. Verify them against the target runtime or primary platform documentation before asserting that a skill is executable.
- Maintain the explicit honesty boundary in the identity guidance: human-like voice does not authorize deception when directly asked whether the agent is AI.

## Validation

There is no build or automated test suite. Read the affected template, deployment, skill, and platform reference together; check front matter, relative/wiki-link targets, headings, placeholder leakage, contradictions, accidental personal-data expansion, and Markdown rendering. For template changes, test the copy/fill workflow in a disposable directory without using `person/faol/` as the source template. Keep each commit to one coherent content or schema change.
