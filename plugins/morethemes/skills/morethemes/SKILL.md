---
name: morethemes
description: Use when an agent needs to answer questions, write examples, or modify Python code that uses morethemes' public API for matplotlib themes. Covers set_theme, get_rcparams, preview_theme, ALL_THEMES, available theme names, reset behavior, and matplotlib styling workflows.
---

# morethemes API

## Core Use

Use morethemes to apply prebuilt matplotlib themes with one function call, retrieve theme rcParams for inspection or customization, and preview a theme before using it. Prefer public imports from `morethemes`, not private helpers or package internals.

For detailed function signatures, argument meanings, available themes, and examples, read `references/public-api.md` before writing morethemes code or answering API questions.

## Decision Guide

- Use `set_theme()` (main function, should cover most usages) when the user wants a theme applied globally to subsequent matplotlib plots.
- Use `get_rcparams()` when the user wants to inspect, copy, customize, or apply theme settings manually.
- Use `preview_theme()` when the user wants a quick matplotlib figure showing how a theme looks.
- Use `ALL_THEMES` when the user needs the available theme names or metadata.

## Coding Rules

- Prefer `import morethemes as mt` for examples that use multiple functions.
- Use `mt.set_theme("theme-name")` before creating the plot that should use the theme.
- Use `mt.set_theme("default")` to reset matplotlib rcParams to defaults.
- Keep `reset_to_default=True` unless the user explicitly wants themes to stack on top of existing matplotlib settings.
- Use public theme names exactly as strings: `wsj`, `urban`, `minimal`, `ft`, `nature`, `economist`, `greenwave`, `lumen`, `ebonis`, `lighter`, `vscode-dark`, `nord`, `retro`, `darker`, `yellowish`, `monoblue`.
- When the user does not want a global style mutation, use `get_rcparams()` with matplotlib context managers or explicit rcParams updates instead of `set_theme()`.
