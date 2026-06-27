# pypalettes Public API

This reference covers pypalettes `0.2.1`.

Official docs:

- `https://y-sunflower.github.io/pypalettes/`
- `https://python-graph-gallery.com/color-palette-finder/`

## Imports

Use stable top-level imports:

```python
from pypalettes import load_palette, load_cmap, create_cmap, show_cmap
```

The package also exports deprecated helpers: `get_source`, `get_hex`, `get_rgb`, `get_kind`, and `add_cmap`. Avoid them in new code unless the user is maintaining older pypalettes code.

## `load_palette()`

```python
load_palette(
    name: str | list[str] = "random",
    reverse: bool = False,
    keep_first_n: int | None = None,
    keep_last_n: int | None = None,
    keep: list[bool] | None = None,
    repeat: int = 1,
    shuffle: bool | int = False,
    remove: int | list[int] | None = None,
) -> list[str]
```

Loads colors from one of the available palettes and returns a list of color strings.

Use it when a library wants a palette/list, for example seaborn categorical palettes:

```python
import seaborn as sns
from pypalettes import load_palette

palette = load_palette("Fun")

sns.lmplot(
    data=sns.load_dataset("penguins"),
    x="bill_length_mm",
    y="bill_depth_mm",
    hue="species",
    palette=palette,
)
```

## `load_cmap()`

```python
load_cmap(
    name: str | list[str] = "random",
    cmap_type: str = "discrete",
    reverse: bool = False,
    keep_first_n: int | None = None,
    keep_last_n: int | None = None,
    keep: list[bool] | None = None,
    repeat: int = 1,
    shuffle: bool | int = False,
    remove: int | list[int] | None = None,
) -> LinearSegmentedColormap | ListedColormap
```

Loads a matplotlib colormap from an available palette.

- `cmap_type="continuous"` returns a continuous colormap.
- `cmap_type="discrete"` returns a listed/discrete colormap.
- Invalid `cmap_type` values raise `ValueError`.

Use it for matplotlib and seaborn arguments named `cmap`:

```python
import matplotlib.pyplot as plt
import numpy as np
from pypalettes import load_cmap

cmap = load_cmap("Sunset2", cmap_type="continuous")
data = np.random.randn(20, 20)

plt.imshow(data, cmap=cmap)
plt.colorbar()
```

Returned colormaps include convenience attributes populated by pypalettes: `source`, `kind`, `hex`, `colors`, `rgb`, `yiq`, and `hsv`.

## Palette Customization

`load_palette()` and `load_cmap()` share customization arguments:

- `reverse=True`: reverse the palette order.
- `keep_first_n=3`: keep only the first 3 colors.
- `keep_last_n=3`: keep only the last 3 colors.
- `keep=[True, False, True]`: keep colors selected by a boolean mask.
- `repeat=2`: repeat the palette in the output.
- `shuffle=True`: shuffle colors.
- `shuffle=42`: shuffle colors with a deterministic seed.
- `remove=2`: remove the third color.
- `remove=[1, 3]`: remove the second and fourth colors.

Example:

```python
from pypalettes import load_palette

palette = load_palette("Fun", reverse=True, keep_first_n=4, shuffle=42)
```

## `create_cmap()`

```python
create_cmap(
    colors: list,
    cmap_type: str = "discrete",
    name: str = "custom_cmap",
) -> LinearSegmentedColormap | ListedColormap
```

Creates a matplotlib colormap from user-provided matplotlib-compatible colors.

```python
import matplotlib.pyplot as plt
import numpy as np
from pypalettes import create_cmap

cmap = create_cmap(
    colors=["#D57A6DFF", "#E8B762FF", "#9CCDDFFF", "#525052FF"],
    cmap_type="continuous",
)

x = np.linspace(0, 20, 1000)
y = np.sin(x)

plt.scatter(x, y, c=y, cmap=cmap)
plt.colorbar()
```

## `show_cmap()`

```python
show_cmap(
    *args,
    max_cols: int = 8,
    spacing: float = 0.1,
    square_size: float = 1,
) -> matplotlib.figure.Figure
```

Shows the colors from a pypalettes colormap as a grid of squares. Positional arguments are passed to `load_cmap()`.

```python
from pypalettes import show_cmap

fig = show_cmap("Sunset2", max_cols=6)
```

Use `max_cols`, `spacing`, and `square_size` to control the preview layout.
