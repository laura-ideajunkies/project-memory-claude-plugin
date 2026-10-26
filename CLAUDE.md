# project-memory plugin

Open-source Claude plugin: three skills (project-init, compound-learning, pm-reflect) that keep per-project memory in plain markdown files.

## Working instructions

- Run the `anthropic-skills:ai-ism-checker` skill on anything added to this repo before committing: README, SKILL.md files, and the commit message itself. Fix what it flags in text you wrote; call out false positives rather than rewording around them.
- UK English spelling throughout, except product UI labels quoted as Anthropic spells them (for example "Customize").
- When a SKILL.md changes, rebuild its package: from `skills/`, `zip -X -r ../dist/<name>.skill <name>/SKILL.md`, and check the zip matches the source.
- Keep `plugin.json` and `marketplace.json` versions in step.
