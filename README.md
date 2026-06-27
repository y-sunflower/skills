# Agent Skills

A list of public skills for Yellow Sunflower open source projects.

<br>

## Skills available

Click on the project to get installation instruction of the skill:

- [`ninejs`](#ninejs): Bringing interactivity to plotnine. ninejs adds interactive behavior to plotnine charts with a minimal API. You can attach tooltips, hover grouping, and on click events, then export the result as a standalone HTML plot.
- [`pypalettes`](#pypalettes): A dependency-free large (+2500) collection of colormaps and palettes for Python.
- [`pyfonts`](#pyfonts): A simple and reproducible way of using fonts in matplotlib via Google font and Bunny font.

<br>


## Usage

Once installed, your agent will **automatically** know when to use the skill, or you can mention it in a prompt using the `$skill-name` syntax:

```bash
Use $ninejs to make my plotnine chart interactive.
```

```bash
Use $pyfonts to use better fonts for this chart.
```

<br>

## Installation

### ninejs

- Claude Code

```bash
claude plugin marketplace add y-sunflower/skills && claude plugin install ninejs@y-sunflower-skills
```

- Codex

```bash
codex plugin marketplace add y-sunflower/skills && codex plugin add ninejs@y-sunflower-skills
```

---

### pyfonts

- Claude Code

```bash
claude plugin marketplace add y-sunflower/skills && claude plugin install pyfonts@y-sunflower-skills
```

- Codex

```bash
codex plugin marketplace add y-sunflower/skills && codex plugin add pyfonts@y-sunflower-skills
```

---

### pypalettes

- Claude Code

```bash
claude plugin marketplace add y-sunflower/skills && claude plugin install pypalettes@y-sunflower-skills
```

- Codex

```bash
codex plugin marketplace add y-sunflower/skills && codex plugin add pypalettes@y-sunflower-skills
```
