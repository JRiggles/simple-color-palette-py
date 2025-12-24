# simple-color-palette-python
v0.1.0

### A Python package for the [Simple Color Palette](https://simplecolorpalette.com) format — a minimal JSON-based file format for defining color palettes (MIT license)

## Latest Changes
- Initial commit

## Install

> TODO: publish to package index

<!-- ```sh
pip install simple-color-palette
``` -->

## API

> TODO

- `Palette` dataclass
- `Color` dataclass
- `Components` dataclass
- `save` function
- `load` function
- `from_hex` function

## Usage

```python
import simplecolorpalette as scp

# define some colors - scp.Color dataclass objects
red_color = scp.Color(
	name='Red',
    components=scp.Components(red=1.0, green=0.0, blue=0.0,)
)

yellow_color = scp.Color(
    name='Yellow',
    components=scp.Components(red=1.0, green=1.0, blue=0.0)
)

green_color = scp.Color(
    name='Green',
    components=scp.Components(red=0.0, green=1.0, blue=0.0,)
)

# define the color palette - scp.Palette object
palette = scp.Palette(
	name='Traffic Lights',
	colors=[red_color, yellow_color, green_color],
)

# introspecting color components
print(red_color.components)
# >>> Components(red=1.0, green=0.0, blue=0.0, opacity=1.0)

# modifying color components
red_color.components.red = 0.9

# save to JSON (as a *.color-palette file)
palette_file_path = 'path/to/palette.color-palette'
scp.save(palette, palette_file_path)

# load palette data from a *.color-palette file
palette_data = scp.load('path/to/palette.color-palette')
print(palette_data)
# >>> Palette(colors=[Color(components=Components(red=0.9, green=0.0, blue=0.0, opacity=1.0), name='Red'), Color(components=Components(red=1.0, green=1.0, blue=0.0, opacity=1.0), name='Yellow'), Color(components=Components(red=0.0, green=1.0, blue=0.0, opacity=1.0), name='Green')], name='Traffic Lights')
```

> [!NOTE]
> - The `simplecolorpalette.save` and `simplecolorpalette.load` functions will accept file paths with either a `*.color-palette` or `*.json` file extension, other file extensions will raise a `ValueError`.
> - Output files saved with `save()` will use the `*.color-palette` extension.


If you want the palette data in dictionary (`dict`) format...
```python
from dataclasses import asdict

...  # other stuff omitted for brevity

print(asdict(palette_data))
# >>> {'colors': [{'components': {'red': 0.9, 'green': 0.0, 'blue': 0.0, 'opacity': 1.0}, 'name': 'Red'}, {'components': {'red': 1.0, 'green': 1.0, 'blue': 0.0, 'opacity': 1.0}, 'name': 'Yellow'}, {'components': {'red': 0.0, 'green': 1.0, 'blue': 0.0, 'opacity': 1.0}, 'name': 'Green'}], 'name': 'Traffic Lights'}
```

---

If you want to define colors using hex codes, you can convert hex strings into color `Components` using the `from_hex` function.

Hex color codes with 3, 4, 6, or 8 digits are allowed, e.g.:
- #B0B
- #FACE
- #2FACED
- #0DEADA55

The leading `#` is ignored

```python
from simplecolorpalette import from_hex


hex_color = '#ff3344'
components = from_hex(hex_color)
print(components)
# >>> Components(red=1.0, green=0.2, blue=0.26667, opacity=1.0)

hex_color_with_opacity = '#4466ffcc'
components_with_opacity = from_hex(hex_color_with_opacity)
print(components_with_opacity)
# >>> Components(red=0.26667, green=0.4, blue=1.0, opacity=0.8)
