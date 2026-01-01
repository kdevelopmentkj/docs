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

# Xtarget

{% embed url="https://kdev-jh.tebex.io/package/xtarget" %}

**Xtarget** is a powerful and flexible context menu and targeting system for your FiveM servers. Designed to be performant, easy to integrate, and highly customizable, it allows you to create dynamic context menus that adapt based on what players are targeting.

_All menu operations are fully protected via **pcall**, ensuring that unexpected errors in user-defined callbacks never disrupt the execution flow. If an error occurs during menu creation, the system automatically **rolls back** any items created in that callback using instanceUUID, preventing corrupted menu states and ensuring data integrity._

> **"Why limit yourself to static menus when you can create dynamic, context-aware interactions? Xtarget: Intelligent menus that adapt to what you're targeting."**

#### ✨ Key Features

* **Dynamic Menu Types** - Automatically detects and displays menus based on entity type (Vehicle, Ped, Player, Object, Networked, Coordinates, Void)
* **Flexible Item System** - Support for basic items, checkboxes, separators, and nested submenus
* **Real-time Updates** - `toLoop` function allows items to update dynamically without closing the menu
* **Event Handling** - Full support for `onRelease`, `onHoverIn`, `onHoverOut`, `onCheckedChanged` with automatic error protection
* **Dynamic Modifications** - Change item properties on-the-fly using `setText`, `setChecked`, `setVisible`, `setDisabled`, `setClickable`
* **Menu Refresh** - Refresh menu display after modifying items using `refreshMenu()` to update widths and positions
* **Item Identification** - Find items by `customIdentifier` or UUID for advanced menu manipulation
* **Adaptive Menu Width** - Menu width automatically adjusts based on content length
* **Checkbox Control** - Choose between automatic or manual toggle behavior with `autoToggle`
* **Clickable Control** - Control item clickability with `clickable` property (non-clickable items can still be hovered)
* **Visual State Priority** - Smart rendering system with priority: disabled > not clickable > clickable
* **Restriction System** - Conditionally show/hide items or entire menus using `isRestricted`
* **Smart Menu Management** - Automatic submenu closing when parent items become disabled or invisible
* **Close All Support** - `closeOnClick` option closes all menus including submenus

#### 🎯 Menu Types

* **Vehicle** - Displayed when targeting vehicles
* **Ped** - Displayed when targeting NPCs (non-players)
* **Player** - Displayed when targeting other players
* **Object** - Displayed when targeting objects/props
* **Networked** - Displayed when targeting networked entities
* **Coord** - Displayed when targeting ground/walls (coordinates)
* **Void** - Displayed when not targeting anything specific

***

**Dependencies :**

* None - Standalone system
