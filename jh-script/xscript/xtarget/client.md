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

# Client

```lua
local Xtarget = exports.Xtarget:getSharedObject()
```

***

```lua
Xtarget.register(callback, options)
```

* callback: <kbd>function</kbd>
  * menuManager: <kbd>function</kbd> - Menu manager instance
  * staticRaycastResult: <kbd>table</kbd> - Raycast result data
  * menuType: <kbd>table</kbd> - Detected menu type flags
* options: <kbd>table</kbd> (optional)
  * isRestricted: <kbd>function</kbd> (optional) - Function that returns boolean to restrict menu visibility
  * saveOnMemory: <kbd>boolean</kbd> (optional) - Save items in memory for faster access
* **return** uuid: `string` - Unique identifier for unregistering

***

```lua
Xtarget.unregister(uuid)
```

* uuid: `string` - The UUID returned by register()
* **return** success: `boolean`

***

## menuManager

The menuManager function is passed to your callback and provides methods to build your menu.

```lua
menuManager(parentMenu?)
```

* parentMenu: <kbd>UUID/table</kbd> (optional) - Parent menu/submenu UUID to add items to
* **return** manager: `table` - Menu manager object

### menuManager Methods

```lua
menuManager().addBasicItem(options)
```

* options: <kbd>table</kbd>
  * text: <kbd>string</kbd>
  * customIdentifier: <kbd>string</kbd> (optional)
  * closeOnClick: <kbd>boolean</kbd> (optional)
  * visible: <kbd>boolean</kbd> (optional, default: true)
  * disabled: <kbd>boolean</kbd> (optional, default: false)
  * toLoop: <kbd>function</kbd> (optional) - Called every frame to update the item
  * onRelease: <kbd>function</kbd> (optional) - Called when item is clicked
  * onHoverIn: <kbd>function</kbd> (optional) - Called when mouse hovers over item
  * onHoverOut: <kbd>function</kbd> (optional) - Called when mouse leaves item
  * isRestricted: <kbd>function</kbd> (optional) - Function that returns boolean to restrict item visibility
* **return** uuid: `string` - Item UUID

***

```lua
menuManager().addCheckBoxItem(options)
```

* options: <kbd>table</kbd>
  * text: <kbd>string</kbd>
  * isChecked: <kbd>boolean</kbd>
  * autoToggle: <kbd>boolean</kbd> (optional, default: true) - If false, you must manually toggle with setChecked()
  * customIdentifier: <kbd>string</kbd> (optional)
  * closeOnClick: <kbd>boolean</kbd> (optional)
  * visible: <kbd>boolean</kbd> (optional, default: true)
  * disabled: <kbd>boolean</kbd> (optional, default: false)
  * toLoop: <kbd>function</kbd> (optional) - Called every frame to update the item
  * onCheckedChanged: <kbd>function</kbd> (optional) - Called when checkbox state changes
  * onRelease: <kbd>function</kbd> (optional)
  * onHoverIn: <kbd>function</kbd> (optional)
  * onHoverOut: <kbd>function</kbd> (optional)
  * isRestricted: <kbd>function</kbd> (optional)
* **return** uuid: `string` - Item UUID

***

```lua
menuManager().addSeparatorItem(options)
```

* options: <kbd>table</kbd> (optional)
* **return** uuid: `string` - Item UUID

***

```lua
menuManager().addSubMenu(options)
```

* options: <kbd>table</kbd>
  * text: <kbd>string</kbd>
  * customIdentifier: <kbd>string</kbd> (optional)
  * visible: <kbd>boolean</kbd> (optional, default: true)
  * disabled: <kbd>boolean</kbd> (optional, default: false)
  * isRestricted: <kbd>function</kbd> (optional)
* **return** subMenu: `UUID/table` - Submenu UUID to use with menuManager(subMenu)

***

```lua
menuManager().getItemByCustomIdentifier(customIdentifier)
```

* customIdentifier: `string`
* **return** item: `table` or `nil` - Item interface object

***

```lua
menuManager().getItemByUUID(uuid)
```

* uuid: `string`
* **return** item: `table` or `nil` - Item interface object

***

```lua
menuManager().closeMenu()
```

* **return** void

***

## staticRaycastResult

The staticRaycastResult table contains information about the raycast hit.

* hit: <kbd>boolean</kbd> - Whether the raycast hit something
* hitEntity: <kbd>number</kbd> - Entity handle that was hit (or 0)
* hitCoords: <kbd>vector3</kbd> - Coordinates where the raycast hit
* hitType: <kbd>number</kbd> - Type of entity hit (0 = nothing, 1 = entity, 2 = ped, 3 = vehicle, etc.)

***

## menuType

The menuType table contains boolean flags indicating what type of target was detected.

* Vehicle: <kbd>boolean</kbd> - True if targeting a vehicle
* Ped: <kbd>boolean</kbd> - True if targeting a ped (NPC, not player)
* Player: <kbd>boolean</kbd> - True if targeting a player
* Object: <kbd>boolean</kbd> - True if targeting an object
* Networked: <kbd>boolean</kbd> - True if targeting a networked entity
* coord: <kbd>boolean</kbd> - True if targeting coordinates (ground, wall)
* Void: <kbd>boolean</kbd> - True if not targeting anything

***

## Item Interface

Items returned by `getItemByCustomIdentifier()` or `getItemByUUID()` provide the following methods:

```lua
item.setText(text)
```

* text: `string`
* **return** void

***

```lua
item.setDisabled(disabled)
```

* disabled: `boolean`
* **return** void

***

```lua
item.setVisible(visible)
```

* visible: `boolean`
* **return** void

***

```lua
item.setCloseOnClick(closeOnClick)
```

* closeOnClick: `boolean`
* **return** void

***

```lua
item.setChecked(checked)  -- Checkbox items only
```

* checked: `boolean`
* **return** void

***

### Item Properties

* text: <kbd>string</kbd> - Current text of the item
* UUID: <kbd>string</kbd> - Unique identifier of the item
* disabled: <kbd>boolean</kbd> - Whether the item is disabled
* visible: <kbd>boolean</kbd> - Whether the item is visible
* isChecked: <kbd>boolean</kbd> - Checkbox state (checkbox items only)
