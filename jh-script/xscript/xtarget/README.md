---
hidden: true
---


# Xtarget

{% embed url="https://kdev-jh.tebex.io/package/xtarget" %}

**Xtarget** is an advanced targeting and menu system for FiveM that provides context-aware interaction menus through raycast detection. Designed to be flexible and powerful, it allows you to create dynamic menus that adapt to what players are looking at.

#### 🎯 Context-Aware Targeting

* **Automatic entity detection**: Detects vehicles, peds, players, objects, and coordinates
* **Dynamic menu generation**: Menus are built based on what the player is targeting
* **Type-based filtering**: Show or hide menu items based on target type

#### 🎨 Flexible Menu System

* **Multiple item types**: Basic items, checkboxes, separators, and submenus
* **Real-time updates**: Update item text and states dynamically with `toLoop`
* **Item manipulation**: Get items by UUID or custom identifier for dynamic updates
* **Event callbacks**: onRelease, onHoverIn, onHoverOut for full control

#### ⚡ Performance Optimized

* **Efficient raycast system**: Optimized detection with configurable flags
* **Smart menu building**: Menus are built only when needed
* **Memory management**: Optional memory saving for frequently used items

<details>

<summary>🎮 Features</summary>

**Menu System**

* ✅ Basic menu items with custom callbacks
* ✅ Checkbox items with auto/manual toggle modes
* ✅ Separator items for menu organization
* ✅ Submenus with unlimited nesting depth
* ✅ Real-time item updates with `toLoop` function

**Targeting System**

* ✅ Vehicle detection and interactions
* ✅ Ped/NPC detection and interactions
* ✅ Player detection and interactions
* ✅ Object detection and interactions
* ✅ Coordinate-based interactions (ground, walls)
* ✅ Networked entity support

**Item Management**

* ✅ Get items by UUID
* ✅ Get items by custom identifier
* ✅ Dynamic text updates
* ✅ Dynamic visibility/disabled state
* ✅ Checkbox state management

</details>

***

**Dependence :**

* onesync
