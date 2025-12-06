# Events and Exports

### Exports

***

#### toggleShopMenu

```lua
exports.kclothshop:toggleShopMenu(type, stype)
```

Open the shop menu and specify the default pages and they type

* type: `number` (corresponding to a component number ex : 11 for shirt)  &#x20;
* stype: `string` ("`clothing`" or "`mask`" or "`bproof`")

return: `void`

#### GetConfig

```lua
local Config = exports.kclothshop:GetConfig()
```

Get configuration table from kclothshop

return: `table`

#### generateClothImage

```lua
exports.kclothshop:generateClothImage(sexPed, ref, variation)
```

* sexPed: `string` ("`male`" or "`female`")
* ref: `number` (corresponding to a component number ex : 11 for shirt)
* variation: `number`

return: `string` (formated image url from configurated cdn url)
