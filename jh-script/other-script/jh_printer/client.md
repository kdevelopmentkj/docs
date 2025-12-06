# Client

```lua
exports.JH_Printer:openUi(uiType, imageData?)
```

```lua
TriggerEvent('JH_Printer:OpenUi', uiType, imageData?)
```

```lua
TriggerClientEvent('JH_Printer:OpenUi', source, uiType, imageData?)
```

* **uiType**: `string`
  * "`componentGenerate`"
  *   "`componentPreview`"

      <sup>⚠️</sup> <sup></sup><sup><mark style="color:orange;">**It is not recommended to use "**<mark style="color:orange;"></sup><sup><mark style="color:orange;">**`component Preview`**<mark style="color:orange;"></sup><sup><mark style="color:orange;">**" with Image Data if you are not sure what you are doing!**<mark style="color:orange;"></sup>

<details>

<summary>imageData?: <code>table</code>  only for "<code>componentPreview"</code> </summary>

* url?: `string`
* outputSize: `table`
  * width: `number`
  * height: `number`
* settings: `table`
  * scale: `number`
  * widthScale: `number`
  * heightScale: `number`
  * rotation: `number`
  * positionX: number
  * positionY: `number`
  * flipHorizontal: `boolean`
  * flipVertical: `boolean`
  * borderRadius: `number`
  * options?: `table`
    * glow: `boolean`
    * effect3d: `boolean`
    * showBorder: `boolean`
    * borderColor: `string`
* originalSize: `table`
  * width: `number`
  * height: `number`

</details>
