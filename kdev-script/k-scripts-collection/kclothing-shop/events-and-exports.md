# Exports

KClothshop exposes **five exports** for integration. Internal script events (e.g. purchase handlers) are not part of the public API and may change between versions.

| Export | Side | How to call |
|--------|------|-------------|
| `GetConfig` | shared | `exports.kclothshop:GetConfig()` |
| `toggleShopMenu` | client | `exports.kclothshop:toggleShopMenu(shop, stype)` |
| `generateClothImage` | server | `exports.kclothshop:generateClothImage(sexPed, ref, variation)` |

Item exports are configured during [Installation](installation.md).

***

## Client exports

### toggleShopMenu

```lua
exports.kclothshop:toggleShopMenu(shop, stype)
```

Opens or toggles the clothing shop NUI. Only works on freemode peds (`mp_m_freemode_01` / `mp_f_freemode_01`).

| Parameter | Type | Description |
|-----------|------|-------------|
| `shop` | `number` | Default GTA **component ID** sent to the UI as `clothRef` (e.g. `8` = t-shirt, `1` = mask, `9` = bproof). Matches `Config.shop[].openD`. |
| `stype` | `string` | Shop mode: `"clothing"`, `"mask"`, or `"bproof"`. Controls which panels are shown. |

**Returns:** `void`

If the menu is already open, calling this again closes it and restores the player's skin via skinchanger.

**Example: open full clothing shop on torso:**

```lua
exports.kclothshop:toggleShopMenu(11, 'clothing')
```

**Example: admin command:**

```lua
RegisterCommand('clothshop', function()
    exports.kclothshop:toggleShopMenu(8, 'clothing')
end, false)
```

***

## Shared exports

### GetConfig

```lua
local cfg = exports.kclothshop:GetConfig()
```

Returns the full `Config` table from `shared/sh_config.lua` (shops, prices, URLs, component lists, etc.).

**Returns:** `table`

**Example:**

```lua
local cfg = exports.kclothshop:GetConfig()
print(cfg.price, cfg.url)
```

See [Configuration](configuration.md) for every option.

***

## Server exports

### generateClothImage

```lua
local url = exports.kclothshop:generateClothImage(sexPed, ref, variation)
```

Builds a preview image URL using `Config.url` and the component lists in config.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sexPed` | `string` | `"male"` or `"female"` |
| `ref` | `string` | Component **name** from `Config.listComponents` or `Config.listComponentsProp` (e.g. `"torso"`, `"mask"`, `"helmet"`). This is **not** the numeric GTA component ID. |
| `variation` | `number` | Drawable / variation index |

**Returns:** `string` full image URL

URL format:

* Drawable: `{Config.url}/{sex}_{componentId}_{variation}.webp`
* Prop: `{Config.url}/{sex}_prop_{propId}_{variation}.webp`

**Example:**

```lua
local url = exports.kclothshop:generateClothImage('male', 'torso', 15)
-- e.g. https://cfx-nui-kclothshop/web/dist/images/male_11_15.webp
```

***

## ox_inventory item exports

These are **not** called with `exports.kclothshop:cloth()` directly. ox_inventory invokes them when a player uses a `cloth` or `outfit` item. Wire them in `ox_inventory/data/items.lua` as shown in [Installation](installation.md).
