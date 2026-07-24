# Theme

The theme file is located at `shared/theme.lua` and controls the **look** of the targeting menu. Two render backends share the same file:

* **Native draw** (default) — the menu is drawn with GTA native draw calls. Styled by `Theme.renderer.menu`, `item`, `separator` and `colors`. Sizes are **screen-space fractions** (`0`–`1`).
* **NUI** (when `Config.modeNUI = true`, see [Configuration](configuration.md)) — the menu is rendered in HTML / NUI instead. Styled by `Theme.renderer.nui`. Sizes are in **pixels**.

***

## Colors

```lua
Color(r, g, b, a)
```

Helper that builds a colour. Components are floored; `a` (alpha) defaults to `255`.

* r, g, b: `number` — `0`–`255`
* a: `number` _(optional)_ — `0`–`255` (default: `255`)

A named palette is exposed as `Colors`. Any **unknown** key returns magenta `Color(255, 0, 255)` (so a typo is visible instead of silent).

* **`Colors`** : `table`
  * White `(255,255,255)`, Black `(0,0,0)`, Grey `(60,64,67)`, LightGrey `(75,76,79)`, DarkGrey `(41,42,45)`
  * Red `(255,0,0)`, Green `(0,255,0)`, Blue `(0,0,255)`, Yellow `(255,255,0)`, Cyan `(0,255,255)`, Magenta `(255,0,255)`, Orange `(255,165,0)`
  * Transparent `(0,0,0,0)`

```lua
-- use a named colour, or build your own
Theme.renderer.colors.text = Colors.White
Theme.renderer.colors.progress = Color(120, 200, 120)
```

```lua
color:Alpha(a)
```

Returns a **copy** of an existing colour with a different alpha — handy to reuse a palette entry at another opacity.

* a: `number` — `0`–`255`
* **return** `Color`

```lua
Theme.renderer.colors.background = Colors.Black:Alpha(200)
```

***

## `Theme.renderer` — Native draw

> Sizes are screen-space fractions (`0`–`1`). Leave these untouched unless you know what you are doing.

* **`menu`** : `table`
  * minWidth: `number` — Minimum menu width (default: `0.15`)
  * maxWidth: `number` — Maximum menu width — text wider than this is clamped (default: `0.4`)
  * textPadding: `number` — Horizontal padding around the label (default: `0.01`)
  * checkboxWidth: `number` — Reserved width for the checkbox box (default: `0.02`)
  * submenuArrowWidth: `number` — Reserved width for the submenu arrow (default: `0.02`)
  * hoverTimeout: `number` — Delay (ms) before a hover is registered (default: `250`)
  * alpha: `number` — Background opacity, `0`–`255` (default: `150`)
* **`item`** : `table`
  * height: `number` — Height of a single item row (default: `0.03`)
* **`separator`** : `table`
  * height: `number` — Height of a separator row (default: `0.012`)
  * lineColor: `Color` — Separator line colour (default: `Colors.Grey`)
* **`colors`** : `table`
  * background: `Color` — Item background (default: `Colors.Black`)
  * backgroundHovered: `Color` — Item background when hovered (default: `Colors.LightGrey`)
  * text: `Color` — Normal text (default: `Colors.White`)
  * textDisabled: `Color` — `disabled` item text (default: `Colors.Grey`)
  * border: `Color` — Menu border (default: `Colors.Grey`)
  * textClickable: `Color` — Text of a `clickable = false` item (default: `Colors.White`)
  * progress: `Color` — Fill colour of `addProgressItem` / `addSliderItem` (default: `Color(120, 200, 120)`)

***

## `Theme.renderer.nui` — NUI mode

> Only used when `Config.modeNUI = true`. Sizes are in **pixels**.

* **`nui`** : `table`
  * borderRadius: `number` — Menu corner radius (px) (default: `8`)
  * borderWidth: `number` — Menu border width (px) (default: `2`)
  * minWidth: `number` — Minimum menu width (px) (default: `200`)
  * maxWidth: `number` — Maximum menu width (px) (default: `600`)
  * itemPaddingX: `number` — Horizontal padding inside an item (px) (default: `15`)
  * itemPaddingY: `number` — Vertical padding inside an item (px) (default: `10`)
  * gap: `number` — Gap between items (px) (default: `0`)
  * fontSize: `number` — Item font size (px) (default: `15`)
  * fontFamily: `string` — CSS font stack (default: `"'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif"`)
  * screenMargin: `number` — Minimum distance kept from the screen edges (px) (default: `8`)
  * submenuGap: `number` — Gap between a menu and its open submenu (px) (default: `6`)

> The per-item palette in `Theme.renderer.colors` (text, background, hovered, progress, …) is shared by both backends — the NUI renderer reads the same colours.

***

## `Theme.rebuild()`

The renderer pre-computes its translucent background colours (built from `colors.background`, `colors.backgroundHovered` and `menu.alpha`) once, when `shared/theme.lua` loads. Editing the values **inside that file** therefore needs nothing extra.

If you change any of those values **at runtime**, call `Theme.rebuild()` afterwards so the pre-computed colours are recalculated:

```lua
Theme.renderer.menu.alpha = 220
Theme.renderer.colors.background = Colors.DarkGrey
Theme.rebuild()
```
