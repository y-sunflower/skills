# morethemes Public API

This reference covers the public API exposed by morethemes `0.7.0` on the main branch.

Official docs:

- `https://y-sunflower.github.io/morethemes/`
- `https://github.com/y-sunflower/morethemes`

## Imports

Common import style:

```python
import morethemes as mt
```

Stable top-level imports:

```python
from morethemes import ALL_THEMES, get_rcparams, preview_theme, set_theme
```

## Available Themes

Use these exact theme names:

```python
[
    "wsj",
    "urban",
    "minimal",
    "ft",
    "nature",
    "economist",
    "greenwave",
    "lumen",
    "ebonis",
    "lighter",
    "vscode-dark",
    "nord",
    "retro",
    "darker",
    "yellowish",
    "monoblue",
]
```

`ALL_THEMES` maps each theme name to theme metadata, including a description and its matplotlib rcParams dictionary.

## `set_theme()`

```python
set_theme(theme_name: str, reset_to_default: bool = True) -> None
```

Applies a theme to global matplotlib rcParams.

```python
import matplotlib.pyplot as plt
import morethemes as mt

mt.set_theme("wsj")

fig, ax = plt.subplots()
ax.plot([1, 2, 3], [3, 2, 5])
ax.set_title("Themed chart")
```

Behavior:

- Theme names are case-insensitive.
- `set_theme("default")` resets matplotlib rcParams to default values.
- `reset_to_default=True` resets rcParams before applying the theme.
- `reset_to_default=False` allows the selected theme to stack on top of existing settings.
- The function loads the theme font with pyfonts and applies it as the matplotlib default font.

Use `reset_to_default=False` only when the user explicitly wants to preserve previous matplotlib customizations:

```python
mt.set_theme("urban", reset_to_default=False)
```

## `get_rcparams()`

```python
get_rcparams(theme_name: str) -> dict[str, Any]
```

Returns the rcParams dictionary for a theme without directly applying it.

```python
import matplotlib.pyplot as plt
import morethemes as mt

rcparams = mt.get_rcparams("lumen")

with plt.rc_context(rc=rcparams):
    fig, ax = plt.subplots()
    ax.bar(["A", "B", "C"], [3, 7, 4])
```

Use this for:

- Inspecting theme settings.
- Combining a theme with a few explicit overrides.
- Avoiding long-lived global matplotlib style changes.

If a theme name is not found, `get_rcparams()` raises `KeyError` with close-match suggestions.

## `preview_theme()`

```python
preview_theme(theme=None) -> matplotlib.figure.Figure
```

Returns a matplotlib figure that previews a theme across several chart types.

```python
import morethemes as mt

fig = mt.preview_theme("economist")
```

If `theme` is provided, `preview_theme()` applies the theme first. The preview figure includes scatter, line, horizontal bar, and pie chart examples.

## Customization Pattern

Use `get_rcparams()` when customizing a theme before plotting:

```python
import matplotlib.pyplot as plt
import morethemes as mt

rcparams = mt.get_rcparams("nord").copy()
rcparams["axes.titlesize"] = 18

with plt.rc_context(rc=rcparams):
    fig, ax = plt.subplots()
    ax.plot([1, 2, 3], [2, 5, 4])
    ax.set_title("Customized Nord theme")
```

For global customizations, apply the theme first and then update `plt.rcParams`:

```python
import matplotlib.pyplot as plt
import morethemes as mt

mt.set_theme("minimal")
plt.rcParams["axes.titlesize"] = 16
```
