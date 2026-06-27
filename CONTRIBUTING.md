# Contributing

This repository is a marketplace of agent plugins for Python projects. Each project lives under `plugins/<project-name>/` and should work in both Codex and Claude Code.

## Add a Project

1. Create a plugin folder:

```bash
mkdir -p plugins/<project-name>/.codex-plugin
mkdir -p plugins/<project-name>/.claude-plugin
mkdir -p plugins/<project-name>/skills/<skill-name>/references
```

2. Add the shared plugin manifest at `plugins/<project-name>/.codex-plugin/plugin.json`:

```json
{
  "name": "<project-name>",
  "version": "0.1.0",
  "description": "Help coding agents use the public <project-name> API.",
  "author": {
    "name": "Joseph Barbier"
  },
  "skills": "./skills/",
  "interface": {
    "displayName": "<project-name>",
    "shortDescription": "Use <project-name>.",
    "longDescription": "Guidance for using the public <project-name> API.",
    "developerName": "Joseph Barbier",
    "category": "Productivity",
    "capabilities": [],
    "defaultPrompt": "Use $<skill-name> to help with <project-name>."
  }
}
```

3. Symlink Claude Code to the shared manifest:

```bash
ln -s ../.codex-plugin/plugin.json plugins/<project-name>/.claude-plugin/plugin.json
```

4. Add the skill at `plugins/<project-name>/skills/<skill-name>/SKILL.md`.

Keep the skill focused on the public API users should rely on. Put longer API references, signatures, and examples in `references/` and link to them from `SKILL.md`.

5. Add the project to both marketplace indexes:

- `.agents/plugins/marketplace.json` for Codex.
- `.claude-plugin/marketplace.json` for Claude Code.

Use the same plugin name in both files. Codex uses a structured `source` object; Claude Code uses a relative source string.

6. Update `README.md` with the new project name, description, and install commands.

## Validate

Run these checks before opening a PR:

```bash
python3 -m json.tool .agents/plugins/marketplace.json
python3 -m json.tool .claude-plugin/marketplace.json
python3 -m json.tool plugins/<project-name>/.codex-plugin/plugin.json

claude plugin validate .
claude plugin validate plugins/<project-name>
```

Claude Code may warn that it ignores the shared manifest's `interface` field. That is expected; the shared manifest is intentionally written for both tools.


## Good to know

All projects are open source and should live in this Github org: https://github.com/y-sunflower.

