# Installation

We try our best to have the easiest installation possible, for a plug-and-play FiveM script.

## 1. Download and place the resource

1. Download `kclothshop` and `ox_inventory Rework` from your [CFX Portal](https://portal.cfx.re/assets/granted-assets).
2. Place the folder in your `resources` directory. Do not rename the folder, as the resource name is used in exports and configuration.

## 2. Start order in server.cfg

Add dependencies **before** `kclothshop`:

```cfg
ensure es_extended
ensure ox_lib
ensure ox_inventory
ensure kclothshop
```

{% hint style="warning" %}
The bundle version of **ox_inventory** is required for this script. You receive it when you purchase the **KClothshop Bundle**.
{% endhint %}

## 3. ox_inventory items

Open `ox_inventory/data/items.lua` and add:

```lua
["cloth"] = {
    label = "Cloth",
    weight = 1,
    stack = false,
    consume = 0,
    server = {
        export = 'kclothshop.cloth',
    },
},

["outfit"] = {
    label = "Outfit",
    weight = 1,
    stack = false,
    consume = 0,
    server = {
        export = 'kclothshop.outfit',
    },
},

["outfitbag"] = {
    label = "Outfit Bag",
    weight = 1,
    stack = false,
    consume = 0,
},
```

## 4. Restart the server

Restart your server (or ensure the resources) so `kclothshop` and `ox_inventory` load with the new items.

## 5. Language (optional)

Set the locale in `server.cfg` **before** the server starts (if not using English):

```cfg
setr esx:locale fr
```

Then edit strings in `locales/fr.lua`. See [Configuration → Translations](configuration.md#translations).

## 6. Preview images (overview)

The shop NUI loads clothing previews from `Config.url` (default: `web/dist/images` inside the resource). For a full wardrobe you usually generate your own `.webp` files, host them on a CDN, and set `Config.url` to that base URL.

File naming must match:

| Type | Pattern |
|------|---------|
| Drawable | `{male\|female}_{componentId}_{drawable}.webp` |
| Prop | `{male\|female}_prop_{propId}_{drawable}.webp` |

Example: `male_11_15.webp`, `female_prop_0_3.webp`

See [step 7](#7-generate-preview-images-greenscreen) below.

## 7. Generate preview images (greenscreen)

This step is **optional** and only needed if you want custom or addon clothing previews in the shop.

### Use a dev server not your live server

Generate images on a **local or dedicated dev FiveM server**. **Do not install the screenshot tool on your production server**: remove it from `server.cfg` before going live.

Recommended tool: **[fivem-greenscreener](https://github.com/Bentix-cs/fivem-greenscreener)** (screenshots every GTA clothing piece, props, and vehicles against a greenscreen).

### Install on the dev server only

1. Clone the repository into `resources/` (follow the author’s note: **do not** place it inside a subfolder like `resources/[scripts]`).
2. Install its dependencies (see the repo README):
   * [screenshot-basic](https://github.com/citizenfx/screenshot-basic)
   * yarn
3. In your **dev** `server.cfg` only:

```cfg
ensure screenshot-basic
ensure fivem-greenscreener
```

4. Join the dev server and run the commands from the greenscreener README, for example:
   * `/screenshot`: batch capture of all clothing (long process; leave the client running).
   * `/customscreenshot [component] [drawable/all] [props/clothing] [male/female/both]`: capture one component or range.

To include **texture variations**, set `"includeTextures": true` in the greenscreener `config.json` (generation takes much longer and produces many more files).

Organize or rename the output so each file matches the [naming pattern](#6-preview-images-overview) expected by KClothshop.

### Upload to a CDN (not Discord)

Upload the `.webp` files to a static file host with **stable direct HTTPS URLs**, for example:

* [Fivemanage](https://fivemanage.com/)
* Any CDN or object storage you already use (S3 + CloudFront, Bunny, etc.)

Then set the base URL in `shared/sh_config.lua`:

```lua
Config.url = 'https://exemple.cdn.com/kclothshop/images'
```

Restart `kclothshop` on your **production** server after changing `Config.url`.

{% hint style="danger" %}
**Do not use Discord to host shop images.** Discord attachment/CDN links are a poor fit for FiveM NUI (unstable URLs, rate limits, not meant for hotlinking). Use [Fivemanage](https://fivemanage.com/) or another proper CDN.
{% endhint %}

{% hint style="info" %}
On your **live** server you only need `kclothshop` and `ox_inventory` and a valid `Config.url`. The greenscreener resource must **not** stay installed or started in production.
{% endhint %}

## Next steps

* Tune shops, prices, and blips in [Configuration](configuration.md).
* Integrate from other resources via [Exports](events-and-exports.md).
