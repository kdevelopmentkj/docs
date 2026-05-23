# KClothshop

{% embed url="https://kdev-jh.tebex.io/package/bundle-clothing" %}

{% embed url="https://youtube.com/watch?v=ixVhR6nwxrs" %}

**KClothshop** (`kclothshop`) is a modern clothing shop system for FiveM. Players browse and purchase clothes through a NUI interface, with items stored as `cloth` and `outfit` pieces inside **ox_inventory**.

## Features

* Smooth and intuitive interface to customize your outfit.
* Clothing and outfit items integrated with **ox_inventory** (equip, swap, outfit bag).
* Compatible with custom skins and freemode characters.
* Configurable shop locations on the map (clothing, mask, bproof).
* Multilingual notifications via `locales/*.lua` (NUI text is fixed in the pre-built interface).
* Optimized for high performance.
* Option to serve preview images through a CDN.

## Dependencies

The resource requires the following to be started **before** `kclothshop`:

| Dependency | Notes |
|------------|-------|
| `es_extended` | ESX framework |
| `ox_lib` | Points, markers, notifications |
| `ox_inventory` | Bundle version included with KClothshop purchase |
| `/onesync` | Enabled on the server |
| `/server:6116` | Minimum server build |

## Editable files (escrow)

After purchase, only these files are meant to be edited on your server:

* `shared/*.lua`: main configuration
* `locales/*.lua`: translations

The shop **NUI is pre-built** in `web/dist/` only (no Vue/Nuxt source in the package). See [Installation → step 7](installation.md#7-generate-preview-images-greenscreen) for generating and hosting preview images.

All other Lua files are protected. Use [Exports](events-and-exports.md) to integrate from other resources.

## Documentation

* [Installation](installation.md)
* [Configuration](configuration.md) (shops, prices, translations)
* [Exports](events-and-exports.md)
