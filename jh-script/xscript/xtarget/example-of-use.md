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

# Example of use

***

## Basic Menu Registration

```lua
local Xtarget = exports.Xtarget:getSharedObject()

Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    menuManager().addBasicItem({
        text = 'Hello World',
        onRelease = function(item)
            print('Item clicked!')
        end
    })
end)
```

***

## Vehicle Menu

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    if not menuType.Vehicle then return end
    
    local vehicle = staticRaycastResult.hitEntity
    
    menuManager().addBasicItem({
        text = 'Enter vehicle',
        closeOnClick = true,
        onRelease = function(item)
            TaskWarpPedIntoVehicle(PlayerPedId(), vehicle, -1)
        end
    })
    
    menuManager().addCheckBoxItem({
        text = 'Engine',
        isChecked = GetIsVehicleEngineRunning(vehicle),
        onCheckedChanged = function(item, newValue)
            SetVehicleEngineOn(vehicle, newValue, true, false)
        end
    })
end)
```

***

## Submenu Example

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    local demoMenu = menuManager().addSubMenu({
        text = 'Demo SubMenu',
    })
    
    menuManager(demoMenu).addBasicItem({
        text = 'SubMenu Item 1',
        onRelease = function(item)
            print('Clicked in submenu!')
        end
    })
    
    menuManager(demoMenu).addCheckBoxItem({
        text = 'SubMenu Checkbox',
        isChecked = true,
        onCheckedChanged = function(item, newValue)
            print('SubMenu Checkbox:', newValue)
        end
    })
end)
```

***

## Real-time Updates with toLoop

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    menuManager().addBasicItem({
        text = 'Health: ???',
        customIdentifier = 'health_display',
        disabled = true,
        toLoop = function(item)
            local health = GetEntityHealth(PlayerPedId()) - 100
            local newText = ('Health: %d'):format(health)
            if item.text ~= newText then
                item.setText(newText)
            end
        end
    })
end)
```

***

## Using Custom Identifier

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    -- Add a checkbox
    menuManager().addCheckBoxItem({
        text = 'Target Checkbox',
        customIdentifier = 'checkbox_target',
        isChecked = false,
        onCheckedChanged = function(item, newValue)
            print('Target checkbox:', newValue)
        end
    })
    
    -- Toggle it from another item
    menuManager().addBasicItem({
        text = 'Toggle checkbox',
        onRelease = function(item)
            local target = menuManager().getItemByCustomIdentifier('checkbox_target')
            if target then
                target.setChecked(not target.isChecked)
            end
        end
    })
end)
```

***

## Using UUID

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    -- Add an item and save its UUID
    local itemUUID = menuManager().addBasicItem({
        text = 'Modifiable Item',
        customIdentifier = 'item_modifiable',
        onRelease = function(item)
            print('UUID of this item:', item.UUID)
        end
    })
    
    -- Modify it from another item
    menuManager().addBasicItem({
        text = 'Modify by UUID',
        onRelease = function(item)
            local target = menuManager().getItemByUUID(itemUUID)
            if target then
                target.setText('Modified via UUID!')
            end
        end
    })
end)
```

***

## Player Menu

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    if not menuType.Player then return end
    
    local targetPed = staticRaycastResult.hitEntity
    local targetPlayer = NetworkGetPlayerIndexFromPed(targetPed)
    local targetName = GetPlayerName(targetPlayer)
    
    menuManager().addBasicItem({
        text = 'Player: ' .. (targetName or 'Unknown'),
        disabled = true,
    })
    
    menuManager().addSeparatorItem({})
    
    menuManager().addBasicItem({
        text = 'View profile',
        onRelease = function(item)
            print('Profile of:', targetName)
        end
    })
end)
```

***

## Coordinate Menu

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    if not menuType.coord then return end
    if menuType.Vehicle or menuType.Ped or menuType.Player or menuType.Object then return end
    
    local coords = staticRaycastResult.hitCoords
    
    menuManager().addBasicItem({
        text = 'Copy coords',
        onRelease = function(item)
            local formatted = string.format('vector3(%.2f, %.2f, %.2f)', coords.x, coords.y, coords.z)
            print('Coords:', formatted)
        end
    })
    
    menuManager().addBasicItem({
        text = 'Teleport here',
        closeOnClick = true,
        onRelease = function(item)
            SetEntityCoords(PlayerPedId(), coords.x, coords.y, coords.z)
        end
    })
end)
```

***

## Manual Checkbox Toggle

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    menuManager().addCheckBoxItem({
        text = 'Manual Toggle',
        isChecked = false,
        autoToggle = false,  -- Manual control
        onCheckedChanged = function(item, newValue)
            print('Request change to:', newValue)
            -- You can add validation here
            item.setChecked(newValue)  -- Confirm the change
            print('Confirmed!')
        end
    })
end)
```

***

## Using Clickable Property

The `clickable` property allows you to make items non-clickable while still allowing hover interactions. This is useful for display-only items or items that need to be unlocked.

**Visual Priority:** The rendering system follows this priority:
1. If `disabled` → `textDisabled` color + normal background
2. If not `disabled` AND `not clickable` → `textClickable` color + normal background
3. If `clickable` (and not disabled) → normal text color + normal/hovered background

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    -- Display-only item (non-clickable but can be hovered)
    menuManager().addBasicItem({
        text = 'Information (hover to see details)',
        clickable = false,
        onHoverIn = function(item)
            print('This item shows information but cannot be clicked')
        end
    })
    
    -- Locked item that becomes clickable when condition is met
    menuManager().addBasicItem({
        text = 'Locked Feature',
        customIdentifier = 'locked_feature',
        clickable = false,
        onRelease = function(item)
            print('This should not be called if clickable is false')
        end
    })
    
    -- Unlock button
    menuManager().addBasicItem({
        text = 'Unlock Feature',
        onRelease = function(item)
            local lockedItem = menuManager().getItemByCustomIdentifier('locked_feature')
            if lockedItem then
                lockedItem.setClickable(true)
                lockedItem.setText('Unlocked Feature')
            end
        end
    })
end)
```

***

## Item State Priority

Items follow a visual priority system based on their state:

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    -- Disabled item (highest priority)
    menuManager().addBasicItem({
        text = 'Disabled Item',
        disabled = true,
        -- Will show: textDisabled color + normal background
    })
    
    -- Non-clickable item (medium priority)
    menuManager().addBasicItem({
        text = 'Non-clickable Item',
        clickable = false,
        -- Will show: textClickable color + normal background
        -- Can still be hovered and trigger onHoverIn/onHoverOut
    })
    
    -- Normal clickable item (lowest priority, but active)
    menuManager().addBasicItem({
        text = 'Clickable Item',
        clickable = true,  -- default
        -- Will show: normal text color + normal/hovered background
    })
end)
```