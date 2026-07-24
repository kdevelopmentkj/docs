# Configuration

The configuration file is located at `shared/config.lua` and can be modified directly.

**Available options :**

* **`modeNUI`** : `boolean` — Render backend for the menu.
  * `false` — Native GTA draw (default).
  * `true` — Render the menu in HTML / NUI instead (styled by `Theme.renderer.nui`, see [Theme](theme.md)).
* **`keyBind`** : `table`
  * key: `string` - Key to press to open the targeting menu (default: `'LMENU'`)
  * type: `string` - Input type (default: `'keyboard'`)
  * description: `string` - Description of the keybind (default: `'Xtarget Bind'`)
* **`keyControl`** : `table`
  * interact: `number` - Control ID for interaction (default: `24`)
  * raycast: `number` - Control ID for raycast (default: `25`)
* **`defaultTextIfVoid`** : `table`
  * Array of tables with text to display when targeting void (default: `{[1] = {text = 'No item here'}}`)
* **`raycast`** : `table`
  * flags: `number` - Raycast flags (default: `511`)
  * ignore: `number` - Entities to ignore (default: `0`)
* **`compat`** : `table`
  * ox\_target: `table`
    * enable: `boolean` - Enable ox\_target compatibility (default: `true`)
    * defaultDistance: `number` - Max targeting distance in meters applied to an ox\_target option that does not declare its own `distance` (default: `7.0`)
    * onSubMenu: `table`
      * enable: `boolean` - Group ox\_target items in their own submenu instead of merging them into the main menu (default: `true`)
      * text: `string` - Submenu text (default: `'Ox Target Integration'`)
* **`debug`** : `table`
  * showRaycastOutline: `boolean` - Show raycast outline (default: `false`)
  * showRaycastLine: `boolean` - Show raycast line (default: `false`)
  * showRaycastResult: `boolean` - Show raycast result (default: `false`)
  * showEntityType: `boolean` - Show entity type (default: `false`)
* **`customProcess`** : `function`
  * Called every frame while targeting, before the hit is resolved. Empty by default.
  * Receives `staticRaycastResult` as parameter
* **`customProcessAfterHit`** : `function`
  * Called every frame while targeting, but **only when the raycast actually hit something**. Ships with an example that draws a marker above a ped's head.
  * Receives `staticRaycastResult` as parameter

> Both hooks run inside a `pcall`. If yours raises an error, the error is printed once and **that hook is disabled for the rest of the session** — targeting itself keeps working.
