# Configuration

The configuration file is located at `shared/sh_config.lua` and is the main **editable** Lua file after escrow. Every option below is read at resource start.

You can also retrieve the live table from other resources via `exports.kclothshop:GetConfig()`, see [Exports](events-and-exports.md).

***

## General options

* **`Config.price`** : `number`
  * Price charged when buying a single cloth piece or a full outfit from the shop (cash first, then bank).
  * Default: `500`

* **`Config.url`** : `string`
  * Base URL for clothing preview images shown in the NUI.
  * Default: `'https://cfx-nui-kclothshop/web/dist/images'`
  * Point to an external CDN (e.g. [Fivemanage](https://fivemanage.com/)) or to `web/dist/images` inside the resource. See [Installation → step 7](installation.md#7-generate-preview-images-greenscreen).

* **`Config.duration`** : `number`
  * UI-related timing in milliseconds.
  * Default: `300`

* **`Config.hideHudEvent`** : `string`
  * Client event name triggered when the shop UI opens or closes (used to hide your HUD).
  * Default: `'core:client:togglefullhud'`
  * Adjust this to match your HUD resource.

***

## Component lists

These tables define labels (via locales) and the **item name** keys used by ox_inventory and `generateClothImage`.

* **`Config.listComponents`** : `table`
  * Maps GTA **drawable** component IDs to `{ label, name }`.
  * `label` uses `TranslateCap(...)` text comes from `locales/*.lua` (see [Translations](#translations)).
  * Keys are numeric component IDs (`0`–`11`).

| ID | `name` key | Category |
|----|------------|----------|
| 0 | face | Face |
| 1 | mask | Mask |
| 2 | hair | Hair |
| 3 | arms | Gloves / arms |
| 4 | pants | Pants |
| 5 | bags | Bag |
| 6 | shoes | Shoes |
| 7 | chain | Accessories / chain |
| 8 | tshirt | T-Shirt |
| 9 | bproof | Kevlar / body armor |
| 10 | decals | Decals |
| 11 | torso | Torso / jacket |

* **`Config.listComponentsProp`** : `table`
  * Maps GTA **prop** component IDs to `{ label, name }`.

| ID | `name` key | Category |
|----|------------|----------|
| 0 | helmet | Hats |
| 1 | glasses | Glasses |
| 2 | ears | Ears |
| 6 | watches | Watches |
| 7 | bracelets | Bracelets |

***

## Default outfits

* **`Config.defaultMale`** : `table`
  * Default skinchanger-style keys for male freemode when comparing or resetting outfit purchases.
* **`Config.defaultFemale`** : `table`
  * Same for female freemode.

Keys follow the `component_index` pattern (e.g. `torso_1`, `torso_2`, `helmet_1`, `arms`, `arms_2`). Values matching these defaults are stripped when saving a purchased outfit.

***

## Map blips

* **`Config.blips`** : `table`
  * Blip settings per shop **type** (`open` field on shop entries).

Each entry (`clothing`, `mask`, `bproof`) supports:

| Field | Type | Description |
|-------|------|-------------|
| `active` | `boolean` | Whether blips are created for this shop type |
| `sprite` | `number` | GTA blip sprite ID |
| `scale` | `number` | Blip scale |
| `color` | `number` | Blip color ID |

Defaults:

| Type | sprite | color |
|------|--------|-------|
| `clothing` | 73 | 5 |
| `mask` | 671 | 2 |
| `bproof` | 175 | 2 |

To disable blips for a shop type, set `active = false` on that entry.

***

## Shop locations

* **`Config.shop`** : `array`
  * List of shop zones. Each entry creates a map blip (if enabled) and an interaction point.

| Field | Type | Description |
|-------|------|-------------|
| `label` | `string` | Blip and interaction label |
| `coords` | `vector3` | Shop position |
| `heading` | `number` | Ped / camera heading (reserved for future use) |
| `openD` | `number` | Default **component ID** opened in the UI (`clothRef`). See table below. |
| `open` | `string` | Shop mode: `"clothing"`, `"mask"`, or `"bproof"` |

### `open` shop modes

| Value | UI behavior |
|-------|-------------|
| `clothing` | Full wardrobe: clothing categories + props sidebar |
| `mask` | Mask shop only |
| `bproof` | Body armor (GPB) shop only |

### `openD` component IDs (common values)

| `openD` | Opens on |
|---------|----------|
| `8` | T-Shirt (default clothing shops) |
| `1` | Mask |
| `9` | Kevlar / bproof |
| `11` | Torso / jacket |
| `4` | Pants |
| `6` | Shoes |

### Example: add a custom clothing shop

```lua
{
    label = 'Downtown Clothing',
    coords = vec3(425.0, -800.0, 29.5),
    heading = 90.0,
    openD = 8,
    open = 'clothing',
},
```

### Example: mask shop (template)

```lua
{
    label = 'Mask Shop',
    coords = vec3(-1337.38, -1278.12, 4.86),
    heading = 99.21,
    openD = 1,
    open = 'mask',
},
```

### Example: bproof shop (template)

```lua
{
    label = 'Body Armor Shop',
    coords = vec3(-1256.66, -1447.41, 4.35),
    heading = 22.79,
    openD = 9,
    open = 'bproof',
},
```

***

## Notifications

* **`sendNotifiy(title, msg, notify)`** : `function`
  * Called by the script to show player notifications.
  * Default implementation uses **ox_lib** (`lib.notify`).
  * You can replace this function in `sh_config.lua` to route notifications to your own system.

Parameters:

| Param | Type | Description |
|-------|------|-------------|
| `title` | `string` | Notification title |
| `msg` | `string` | Notification body |
| `notify` | `string` | ox_lib type: `'success'`, `'error'`, `'warning'`, etc. |

***

## Export defined in config

* **`GetConfig`** returns the full `Config` table. Documented in [Exports](events-and-exports.md).

***

## Translations

Translation files live in `locales/` (`en.lua`, `fr.lua`, `de.lua`, `es.lua`). They are loaded automatically with the resource.

### Change the language

Set the ESX locale in `server.cfg`, then restart the server:

```cfg
setr esx:locale fr
```

Do **not** change language by editing `Config.Locale` in `sh_config.lua` that line only mirrors the convar and is not used to switch languages.

### What is translated

| Translated via `locales/` | Not translated (pre-built NUI) |
|---------------------------|--------------------------------|
| ox_lib notifications (purchase errors, ped access, etc.) | Shop sidebar labels (Vêtements, Props, …) |
| Strings passed to `TranslateCap` in Lua | Layout, colors, buttons |

To change NUI text you would need an updated `web/dist` from the author, not a locale file.

### Edit strings

Open the file for your language, for example `locales/fr.lua`:

```lua
Locales["fr"] = {
    ['cant_afford_error'] = 'Vous n\'avez pas assez d\'argent pour acheter cela',
    ['cloakshop_title'] = 'Magasin',
    -- ...
}
```

Keep the **keys** unchanged; only change the text on the right.

### Add a language

1. Copy `locales/en.lua` to `locales/it.lua` (example).
2. Set `Locales["it"] = { ... }` and translate each value.
3. Add `setr esx:locale it` in `server.cfg`.
4. Restart the server.

### Locale keys

**Notifications**

| Key | English |
|-----|---------|
| `cant_afford_error` | You don't have enough money to buy this |
| `cloakshop_title` | Shop |
| `peds_access_error` | Your ped doesn't have access to this menu |
| `select_cloth_error` | Please select a piece of clothing |
| `limit_reached` | You have reached the limit |
| `not_avaible` | Feature not available |
| `incompatible_character_model` | Incompatible character model (add to `en`/`de`/`es` if missing included in `fr`) |
| `purchase_error` | Used by the script on purchase failure (add to your locale file if you see the raw key in-game) |

**Component labels** (used in `Config.listComponents` / `Config.listComponentsProp`)

`component_face`, `component_mask`, `component_hair`, `component_arms`, `component_legs`, `component_bag`, `component_shoes`, `component_accessory`, `component_tshirt`, `component_kevlar`, `component_decals`, `component_torso`, `component_hats`, `component_glasses`, `component_ears`, `component_watches`, `component_bracelets`

Copy every key from `locales/en.lua` when creating a new language file.
