# pyfonts Public API

## Purpose

pyfonts provides a simple, reproducible way to use custom fonts in matplotlib. It loads Google Fonts, Bunny Fonts, arbitrary remote font files, or local font files as `matplotlib.font_manager.FontProperties` objects.

Public imports:

```python
from pyfonts import (
    load_font,
    load_google_font,
    load_bunny_font,
    set_default_font,
    preview_font,
    clear_pyfonts_cache,
)
```

## Function Relationships

- `load_google_font()` and `load_bunny_font()` resolve a provider CSS/API request to a concrete font file URL, then call `load_font()`.
- `load_font()` is the base loader. It accepts a direct font file URL or local path and returns `FontProperties`.
- Provider loaders attach internal provider metadata to the returned `FontProperties`. `set_default_font()` uses that metadata to best-effort register related weight/style variants with matplotlib.
- `set_default_font()` updates matplotlib `rcParams` so future labels, ticks, legends, titles, and text use the loaded font by default.
- `preview_font()` is a small convenience wrapper around `load_font()` that returns a matplotlib `Figure`.
- `clear_pyfonts_cache()` clears cached downloaded font files and provider URL cache data.

## Choosing a Loader

Use `load_google_font()` for named families from Google Fonts:

```python
import matplotlib.pyplot as plt
from pyfonts import load_google_font

font = load_google_font("Roboto", weight="bold")
fig, ax = plt.subplots()
ax.text(0.5, 0.5, "Hello", font=font, ha="center")
```

Use `load_bunny_font()` for named families from Bunny Fonts:

```python
from pyfonts import load_bunny_font

font = load_bunny_font("Barrio")
```

Use `load_font()` for a direct font file URL or local font file:

```python
from pyfonts import load_font

font = load_font("https://github.com/y-sunflower/pyfonts/blob/main/tests/Ultra-Regular.ttf?raw=true")
local_font = load_font("fonts/MyFont.ttf")
```

## `load_font()`

Signature:

```python
load_font(
    font_url: Optional[str] = None,
    use_cache: bool = True,
    danger_not_verify_ssl: bool = False,
    font_path: Optional[str] = None,
) -> FontProperties
```

Arguments:

- `font_url`: Direct URL to a binary font file, or a local font file path. Required unless using deprecated `font_path`.
- `use_cache`: Cache downloaded remote font files. Defaults to `True`.
- `danger_not_verify_ssl`: If `True`, retry SSL certificate failures without verification. This is insecure; use only when explicitly requested for trusted local/proxy/firewall environments.
- `font_path`: Deprecated local path argument. Prefer `load_font("path/to/font.ttf")`.

Behavior:

- Non-URL `font_url` values are treated as local paths.
- GitHub URLs must point to raw binary font content, typically with `?raw=true`.
- Remote files are cached under the user's cache directory.
- `.woff` and `.woff2` files are decompressed to `.ttf` for matplotlib compatibility when needed.
- Returns `matplotlib.font_manager.FontProperties`.

Common errors:

- Missing `font_url` raises `ValueError`.
- Missing local files raise `FileNotFoundError`.
- Non-raw GitHub URLs raise `ValueError` with guidance to append `?raw=true`.

## `load_google_font()`

Signature:

```python
load_google_font(
    family: str,
    weight: Optional[Union[int, str]] = None,
    italic: Optional[bool] = None,
    allowed_formats: list[str] = ["woff2", "woff", "ttf", "otf"],
    subset: str = "latin",
    use_cache: bool = True,
    danger_not_verify_ssl: bool = False,
) -> FontProperties
```

Arguments:

- `family`: Google Fonts family name, such as `"Roboto"` or `"Open Sans"`.
- `weight`: Numeric weight such as `400` or `700`, or one of `"thin"`, `"extra-light"`, `"light"`, `"regular"`, `"medium"`, `"semi-bold"`, `"bold"`, `"extra-bold"`, `"black"`.
- `italic`: `True` for italic, `False` for non-italic, `None` for provider default.
- `allowed_formats`: Preferred file formats in order. Defaults to `["woff2", "woff", "ttf", "otf"]`.
- `subset`: Unicode subset, such as `"latin"` or `"thai"`. Defaults to `"latin"`.
- `use_cache`: Cache provider URL lookup and downloaded font file. Defaults to `True`.
- `danger_not_verify_ssl`: Forwarded to `load_font()`.

Use this for the common case where the user knows a Google Fonts family name.

## `load_bunny_font()`

Signature:

```python
load_bunny_font(
    family: str,
    weight: Optional[Union[int, str]] = None,
    italic: Optional[bool] = None,
    allowed_formats: list[str] = ["woff", "ttf", "otf"],
    subset: str = "latin",
    use_cache: bool = True,
    danger_not_verify_ssl: bool = False,
) -> FontProperties
```

Arguments mostly match `load_google_font()`.

Differences:

- Uses Bunny Fonts instead of Google Fonts.
- Default `allowed_formats` excludes `"woff2"` because Bunny's woff2 support may require brotli support.
- Prefer this when the user requests Bunny Fonts or a GDPR-compliant alternative to Google Fonts.

## `set_default_font()`

Signature:

```python
set_default_font(font: FontProperties) -> None
```

Use after loading a font to make it the default for subsequent matplotlib text:

```python
import matplotlib.pyplot as plt
from pyfonts import load_google_font, set_default_font

set_default_font(load_google_font("Fascinate Inline"))
plt.title("Title")
plt.xlabel("X axis")
```

Behavior:

- Registers the loaded font file with matplotlib.
- Updates relevant `matplotlib.rcParams` including family, style, weight, size, stretch, and variant.
- For fonts loaded from Google Fonts or Bunny Fonts, attempts to register related weight/style variants so later matplotlib overrides like `fontweight="bold"` or `fontstyle="italic"` can resolve.

## `preview_font()`

Signature:

```python
preview_font(font_url: str) -> matplotlib.figure.Figure
```

Loads a direct font URL/path with `load_font()` and returns a `Figure` containing sample text.

```python
from pyfonts import preview_font

fig = preview_font("https://github.com/y-sunflower/pyfonts/blob/main/tests/Ultra-Regular.ttf?raw=true")
```

Use it for quick visual checks, not for provider family names.

## `clear_pyfonts_cache()`

Signature:

```python
clear_pyfonts_cache(verbose: bool = True) -> None
```

Clears:

- Downloaded font files in the pyfonts cache directory.
- The provider URL cache file.
- The in-memory provider URL cache.

Use `clear_pyfonts_cache(verbose=False)` in tests or quiet scripts.

## Common Matplotlib Patterns

Pass the returned font to individual text elements:

```python
font = load_google_font("Roboto", weight=700)
ax.set_title("Revenue", font=font)
ax.text(0.5, 0.5, "Label", font=font)
```

Set a global default before creating labels/titles:

```python
font = load_bunny_font("Open Sans", weight="regular")
set_default_font(font)
```

Use subsets for non-Latin text when the provider supports them:

```python
font = load_google_font("Noto Sans Thai", subset="thai")
```

## Public API Boundary

Do not present private helpers as user-facing API:

- `_get_fonturl`
- `_attach_font_provider_metadata`
- `_get_font_provider_metadata`
- `_create_cache_from_fontfile`
- `_decompress_woff_to_ttf`
- `_is_url`
- `_is_valid_raw_url`

Use them only when editing pyfonts internals or tests.
