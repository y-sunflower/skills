---
name: pyfonts
description: Use when Codex needs to answer questions, write examples, or modify Python code that uses pyfonts' public API for matplotlib fonts. Covers what pyfonts does, which public functions to import, available arguments/options, return values, provider-specific behavior for Google Fonts and Bunny Fonts, cache handling, previewing fonts, setting matplotlib defaults, and how these functions relate to each other.
---

# Pyfonts API

## Core Use

Use pyfonts to load remote or local font files as `matplotlib.font_manager.FontProperties` objects for matplotlib text. Prefer public imports from `pyfonts`, not private helpers or module internals.

For detailed function signatures, argument meanings, relationships, and examples, read `references/public-api.md` before writing pyfonts code or answering API questions.

## Decision Guide

- Use `load_google_font()` when the font family is available on Google Fonts.
- Use `load_bunny_font()` when the font family is available on Bunny Fonts or when a GDPR-friendly Google Fonts alternative is preferred.
- Use `load_font()` when the user has a direct font file URL or a local font file path.
- Use `set_default_font()` after loading a font when all matplotlib text should use it by default.
- Use `preview_font()` when the user wants a quick matplotlib figure showing a direct font URL/path.
- Use `clear_pyfonts_cache()` when cached font files or provider URL lookups should be cleared.

## Coding Rules

- Import from `pyfonts`: `from pyfonts import load_google_font, load_bunny_font, load_font, set_default_font, preview_font, clear_pyfonts_cache`.
- Pass the returned `FontProperties` object to matplotlib text with `font=font`.
- For GitHub font file URLs, ensure the URL points to raw binary content, usually by appending `?raw=true`.
- Keep `danger_not_verify_ssl=False` unless the user explicitly asks to bypass SSL verification in a trusted environment.
