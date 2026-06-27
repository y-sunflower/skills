# Agent Skills

A list of public plugins and skills for Yellow Sunflower open source projects.

<br>

## Plugins

- `ninejs`: helps coding agents use the public `ninejs` API to add browser interactivity to plotnine charts.
- `pyfonts`: helps coding agents use the public `pyfonts` API for matplotlib fonts.

<br>

## Install

First-time install, as one shell command:

```bash
codex plugin marketplace add y-sunflower/skills && codex plugin add ninejs@y-sunflower-skills
codex plugin marketplace add y-sunflower/skills && codex plugin add pyfonts@y-sunflower-skills

claude plugin marketplace add y-sunflower/skills && claude plugin install ninejs@y-sunflower-skills
claude plugin marketplace add y-sunflower/skills && claude plugin install pyfonts@y-sunflower-skills
```

Or add this repository as a plugin marketplace once:

```bash
codex plugin marketplace add y-sunflower/skills
claude plugin marketplace add y-sunflower/skills
```

Then install either plugin by name:

```bash
codex plugin add ninejs@y-sunflower-skills
codex plugin add pyfonts@y-sunflower-skills

claude plugin install ninejs@y-sunflower-skills
claude plugin install pyfonts@y-sunflower-skills
```

<br>

## Usage

Mention the installed skill in a prompt:

```text
Use $ninejs to make my plotnine chart interactive.
Use $pyfonts-api to help style this matplotlib chart with pyfonts.
```
