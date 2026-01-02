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

* callback: `function`
  * menuManager: `function` - Menu manager instance
  * staticRaycastResult: `table` - Raycast result data
  * menuType: `table` - Detected menu type flags
* options: `table` (optional)
  * isRestricted: `function` (optional) - Function that returns boolean to restrict menu visibility
  * saveOnMemory: `boolean` (optional) - Save items in memory for faster access
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

* parentMenu: `UUID/table` (optional) - Parent menu/submenu UUID to add items to
* **return** manager: `table` - Menu manager object

### menuManager Methods

```lua
menuManager().addBasicItem(options)
```

* options: `table`
  * text: `string` or `function` - Static text or function returning text (for dynamic refresh)
  * customIdentifier: `string` (optional)
  * closeOnClick: `boolean` (optional)
  * visible: `boolean` or `function` (optional, default: true) - Static or function returning boolean
  * disabled: `boolean` or `function` (optional, default: false) - Static or function returning boolean
  * clickable: `boolean` or `function` (optional, default: true) - Static or function returning boolean. If false, item cannot be clicked but can still be hovered
  * toLoop: `function` (optional) - Called every frame to update the item
  * onRelease: `function` (optional) - Called when item is clicked
  * onHoverIn: `function` (optional) - Called when mouse hovers over item
  * onHoverOut: `function` (optional) - Called when mouse leaves item
  * isRestricted: `function` (optional) - Function that returns boolean to restrict item visibility
* **return** uuid: `string` - Item UUID

***

```lua
menuManager().addCheckBoxItem(options)
```

* options: `table`
  * text: `string` or `function` - Static text or function returning text (for dynamic refresh)
  * isChecked: `boolean`
  * autoToggle: `boolean` (optional, default: true) - If false, you must manually toggle with setChecked()
  * customIdentifier: `string` (optional)
  * closeOnClick: `boolean` (optional)
  * visible: `boolean` or `function` (optional, default: true) - Static or function returning boolean
  * disabled: `boolean` or `function` (optional, default: false) - Static or function returning boolean
  * clickable: `boolean` or `function` (optional, default: true) - Static or function returning boolean. If false, item cannot be clicked but can still be hovered
  * toLoop: `function` (optional) - Called every frame to update the item
  * onCheckedChanged: `function` (optional) - Called when checkbox state changes
  * onRelease: `function` (optional)
  * onHoverIn: `function` (optional)
  * onHoverOut: `function` (optional)
  * isRestricted: `function` (optional)
* **return** uuid: `string` - Item UUID

***

```lua
menuManager().addSeparatorItem(options)
```

* options: `table` (optional)
* **return** uuid: `string` - Item UUID

***

```lua
menuManager().addSubMenu(options)
```

* options: `table`
  * text: `string` or `function` - Static text or function returning text (for dynamic refresh)
  * customIdentifier: `string` (optional)
  * visible: `boolean` or `function` (optional, default: true) - Static or function returning boolean
  * disabled: `boolean` or `function` (optional, default: false) - Static or function returning boolean
  * clickable: `boolean` or `function` (optional, default: true) - Static or function returning boolean. If false, submenu cannot be opened but can still be hovered
  * isRestricted: `function` (optional)
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

```lua
menuManager().refreshMenu()
```

* **return** void
* Refreshes the current menu by synchronizing all items, recalculating menu width and positions
* Useful when you modify item properties (text, checked state, etc.) and want to update the menu display

***

```lua
menuManager().addRefresher(uuids)
```

* uuids: `table` - Array of item UUIDs to group together for partial refresh
* **return** groupUUID: `string` - Unique identifier for the refresh group

When an event (onRelease, onHoverIn, etc.) is triggered on any item in the group, all items in the group will have their functional properties (text, visible, disabled, clickable) re-evaluated automatically. Only the items in the group are refreshed, not the entire menu.

***

## staticRaycastResult

The staticRaycastResult table contains information about the raycast hit.

* hit: `boolean` - Whether the raycast hit something
* hitEntity: `number` - Entity handle that was hit (or 0 if nothing)
* hitCoords: `vector3` - Coordinates where the raycast hit
* hitDistanceByCamera: `number` - Distance between the camera and the hit point
* hitDistanceByPlayer: `number` - Distance between the player and the hit point
* hitHeading: `number` - Heading of the hit entity (0 if no entity)
* hitVelocity: `vector3` - Velocity of the hit entity (vec3(0,0,0) if no entity)
* hitType: `number` - Type of entity hit (0 = nothing, 1 = ped, 2 = vehicle, 3 = object)
* hitSurface: `vector3` - Surface normal at the hit point
* hitMaterial: `number` - Material hash at the hit point

***

## menuType

The menuType table contains boolean flags indicating what type of target was detected.

* Vehicle: `boolean` - True if targeting a vehicle
* Ped: `boolean` - True if targeting a ped (NPC, not player)
* Player: `boolean` - True if targeting a player
* Object: `boolean` - True if targeting an object
* Networked: `boolean` - True if targeting a networked entity
* Coord: `boolean` - True if targeting coordinates (ground, wall)
* Void: `boolean` - True if not targeting anything

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
item.setClickable(clickable)
```

* clickable: `boolean` - If false, item cannot be clicked but can still be hovered
* **return** void

***

```lua
item.setChecked(checked)  -- Checkbox items only
```

* checked: `boolean`
* **return** void

***

```lua
item.setAutoToggle(autoToggle)  -- Checkbox items only
```

* autoToggle: `boolean` - Enable or disable automatic toggle behavior
* **return** void

***

### Item Properties

* text: `string` - Current text of the item
* UUID: `string` - Unique identifier of the item
* parentUUID: `string` - UUID of the parent menu/submenu
* disabled: `boolean` - Whether the item is disabled
* visible: `boolean` - Whether the item is visible
* clickable: `boolean` - Whether the item can be clicked
* checked: `boolean` - Checkbox state (checkbox items only)
* autoToggle: `boolean` - Auto toggle behavior (checkbox items only)