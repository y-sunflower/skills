---
name: ninejs
description: Use the public ninejs Python API to add browser interactivity to plotnine charts. Use when an agent needs to write or explain code with `interactive`, `css`, `javascript`, `save`, `to_html`, `to_iframe`, `show`, ninejs aesthetic mappings such as `tooltip`, `hover_group`, `hover_key`, or `on_click`, or integrations with notebooks, Quarto, marimo, Shiny for Python, Streamlit, or standalone HTML.
---

# ninejs

## Overview

ninejs wraps plotnine charts as self-contained browser-interactive HTML. Stay on the public API documented here and in `references/public-api.md`; do not rely on package internals unless the user explicitly asks to develop ninejs itself.

Read `references/public-api.md` when exact signatures, selector names, supported geoms, or integration snippets matter.

## Imports

Use only these stable top-level imports:

```python
from ninejs import interactive, css, javascript, save, to_html, to_iframe, show
```

For click effects, import bundled effects explicitly:

```python
from ninejs.effects import confetti
```

## Composition

Start with `interactive(gg)`, optionally add `css(...)` and/or `javascript(...)`, then finish with one terminal action:

```python
interactive(gg) + save("plot.html")
html = interactive(gg) + to_html()
iframe = interactive(gg) + to_iframe()
interactive(gg) + show()
```

`css(...)` and `javascript(...)` are chainable. `save(...)` and `show()` return `None`; `to_html(...)` and `to_iframe(...)` return strings. In JupyterLab and VS Code notebooks, `interactive(gg)` renders automatically when it is the last expression in a cell.

## Interactivity

Put ninejs mappings inside plotnine `aes()`:

- `tooltip="<column>"`: sanitized HTML shown on hover.
- `hover_group="<column>"`: hover one element and highlight elements with the same group.
- `hover_key="<column>"`: link hover effects across axes or subplots.
- `on_click="<column>"`: JavaScript snippet to run when an element is clicked.
- `data_id="<column>"`: backward-compatible alias for `hover_group`; prefer `hover_group`.

Prefer `hover_group` for same-chart grouping and `hover_key` for cross-axis/subplot linking. Empty or missing `on_click` values produce no click handler.

## Options

Use `interactive(gg, hover_nearest=True)` when users need nearest-element hover across a panel. Use `reverse_hover=True` to dim the hovered group instead of dimming everything else. Use `zoomable=True` to enable chart zoom. Pass `savefig_kws` such as `dpi=200` through `interactive(...)` when Matplotlib SVG export options are needed.

Use exactly one source for CSS:

```python
interactive(gg) + css(".tooltip { font-size: 18px; }") + save("plot.html")
interactive(gg) + css(from_dict={".point.hovered": {"stroke": "black"}}) + save("plot.html")
interactive(gg) + css(from_file="style.css") + save("plot.html")
```

Use exactly one source for JavaScript:

```python
interactive(gg) + javascript("console.log('ready')") + save("plot.html")
interactive(gg) + javascript(from_file="script.js") + save("plot.html")
```

Treat `javascript(...)` and `on_click` column values as trusted code that runs directly in the output page.

## Examples

Tooltip and grouped hover:

```python
from plotnine import aes, geom_point, ggplot, theme_minimal
from plotnine.data import anscombe_quartet
from ninejs import css, interactive, save

gg = (
    ggplot(
        anscombe_quartet,
        aes(x="x", y="y", color="dataset", tooltip="dataset", hover_group="dataset"),
    )
    + geom_point(size=7, alpha=0.5)
    + theme_minimal()
)

interactive(gg) + css(from_dict={".tooltip": {"font-size": "2em"}}) + save("plot.html")
```

Click handler:

```python
df["open_url"] = "window.open('https://example.com/')"
gg = ggplot(df, aes("x", "y", tooltip="label", on_click="open_url")) + geom_point()
interactive(gg) + save("plot.html")
```

Shiny for Python:

```python
from shiny import render, ui
from ninejs import interactive, to_iframe

@render.ui
def chart():
    return ui.HTML(interactive(plot()) + to_iframe(height="90%", width="70%"))
```

## Constraints

Use ninejs with supported interactive geoms: points, jittered points, lines, paths, steps, bars, columns, histograms, areas, ribbons, and map polygons. Facets work with `facet_wrap` and `facet_grid`.

When combining Matplotlib with plotnine, mutate the figure returned by `gg.draw()`, then pass the original plotnine object to `interactive(gg)`. ninejs reads interactivity from the plotnine object, not from a Matplotlib `fig`.

Generated HTML is self-contained and browser-only. Tooltip HTML is sanitized with DOMPurify, but custom JavaScript is trusted. `hover_nearest=True` can cost more to initialize on very large or complex charts.
