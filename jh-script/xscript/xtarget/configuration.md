---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Configuration

The configuration file is located at `shared/config.lua` and can be modified directly.

**Available options :**

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
  * ox_target: `table`
    * enable: `boolean` - Enable ox_target compatibility (default: `false`)
    * onSubMenu: `table`
      * enable: `boolean` - Show ox_target items in submenu (default: `false`)
      * text: `string` - Submenu text (default: `'Ox Target Integration'`)

* **`debug`** : `table`
  * showRaycastOutline: `boolean` - Show raycast outline (default: `false`)
  * showRaycastLine: `boolean` - Show raycast line (default: `false`)
  * showRaycastResult: `boolean` - Show raycast result (default: `false`)
  * showEntityType: `boolean` - Show entity type (default: `false`)

* **`customProcess`** : `function`
  * Custom function called every frame when menu is open (default: draws marker on ped head)
  * Receives `staticRaycastResult` as parameter
