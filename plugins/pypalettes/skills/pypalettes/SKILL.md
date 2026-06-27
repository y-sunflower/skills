---
name: pypalettes
description: Use when Codex needs to answer questions, write examples, or modify Python code that uses pypalettes' public API for color palettes and matplotlib colormaps. Covers load_palette, load_cmap, create_cmap, show_cmap, palette customization options, and matplotlib/seaborn usage.
---

# pypalettes API

## Core Use

Use pypalettes to load dependency-free color palettes as lists of color strings, or to create matplotlib colormap objects for matplotlib, seaborn, and other Python plotting libraries. Prefer public imports from `pypalettes`, not private helpers or package internals.

For detailed function signatures, argument meanings, return values, and examples, read `references/public-api.md` before writing pypalettes code or answering API questions.

## Decision Guide

- Use `load_palette()` when the plotting library expects a list of colors, such as seaborn categorical `palette=`.
- Use `load_cmap()` when matplotlib, seaborn, or another library expects a colormap object.
- Use `create_cmap()` when the user already has colors and needs a matplotlib colormap.
- Use `show_cmap()` when the user wants to preview the colors in a pypalettes colormap.

## Coding Rules

- Import from `pypalettes`: `from pypalettes import load_palette, load_cmap, create_cmap, show_cmap`.
- Use `cmap_type="continuous"` for continuous numeric values and image-like data.
- Use `cmap_type="discrete"` for categorical or stepped values.
- Use palette names from the official Color Palette Finder when a specific palette is requested.
- Prefer the customization arguments on `load_palette()` and `load_cmap()` over manual list slicing when reversing, keeping, repeating, shuffling, or removing colors.
- Treat deprecated exports such as `get_source`, `get_hex`, `get_rgb`, `get_kind`, and `add_cmap` as legacy helpers; do not introduce them in new examples unless the user explicitly asks about legacy code.
