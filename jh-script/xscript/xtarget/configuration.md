# Configuration

The configuration file is located at `shared/config.lua` and can be modified directly.

**Available options :**

* **`keyBind`** : <kbd>table</kbd>
  * key: <kbd>string</kbd> - Key to press to open the targeting menu (default: `'LMENU'`)
  * type: <kbd>string</kbd> - Input type (default: `'keyboard'`)
  * description: <kbd>string</kbd> - Description of the keybind (default: `'Xtarget Bind'`)

* **`keyControl`** : <kbd>table</kbd>
  * interact: <kbd>number</kbd> - Control ID for interaction (default: `24`)
  * raycast: <kbd>number</kbd> - Control ID for raycast (default: `25`)

* **`defaultTextIfVoid`** : <kbd>table</kbd>
  * Array of tables with text to display when targeting void (default: `{[1] = {text = 'No item here'}}`)

* **`raycast`** : <kbd>table</kbd>
  * flags: <kbd>number</kbd> - Raycast flags (default: `511`)
  * ignore: <kbd>number</kbd> - Entities to ignore (default: `0`)

* **`compat`** : <kbd>table</kbd>
  * ox_target: <kbd>table</kbd>
    * enable: <kbd>boolean</kbd> - Enable ox_target compatibility (default: `false`)
    * onSubMenu: <kbd>table</kbd>
      * enable: <kbd>boolean</kbd> - Show ox_target items in submenu (default: `false`)
      * text: <kbd>string</kbd> - Submenu text (default: `'Ox Target Integration'`)

* **`debug`** : <kbd>table</kbd>
  * showRaycastOutline: <kbd>boolean</kbd> - Show raycast outline (default: `false`)
  * showRaycastLine: <kbd>boolean</kbd> - Show raycast line (default: `false`)
  * showRaycastResult: <kbd>boolean</kbd> - Show raycast result (default: `false`)
  * showEntityType: <kbd>boolean</kbd> - Show entity type (default: `false`)

* **`customProcess`** : <kbd>function</kbd>
  * Custom function called every frame when menu is open (default: draws marker on ped head)
  * Receives `staticRaycastResult` as parameter
