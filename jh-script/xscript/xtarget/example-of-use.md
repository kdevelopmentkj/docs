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
                target.setChecked(not target.checked)
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

## Refreshing Menu

You can refresh the menu to update the display after modifying item properties.

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    -- Add a dynamic item
    local counterItem = menuManager().addBasicItem({
        text = "Counter: 0",
        customIdentifier = "counter",
        disabled = true
    })
    
    -- Button to increment and refresh
    menuManager().addBasicItem({
        text = "Increment Counter",
        onRelease = function(item)
            local counter = menuManager().getItemByCustomIdentifier("counter")
            if counter then
                local current = tonumber(counter.text:match("%d+")) or 0
                counter.setText("Counter: " .. (current + 1))
                menuManager().refreshMenu()  -- Refresh to update display
            end
        end
    })
    
    -- Button to toggle checkbox and refresh
    local checkbox = menuManager().addCheckBoxItem({
        text = "Feature Enabled",
        isChecked = false,
        customIdentifier = "feature_toggle"
    })
    
    menuManager().addBasicItem({
        text = "Toggle Feature",
        onRelease = function(item)
            local toggle = menuManager().getItemByCustomIdentifier("feature_toggle")
            if toggle then
                toggle.setChecked(not toggle.checked)
                menuManager().refreshMenu()  -- Refresh to update checkbox display
            end
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

***

## Refresh Groups with Dynamic Properties

Refresh groups allow you to define items that automatically update when any item in the group triggers an event (onRelease, onHoverIn, etc.). Use functions instead of static values for properties that need to be re-evaluated.

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    if not menuType.Vehicle then return end
    
    local vehicle = staticRaycastResult.hitEntity
    
    -- Item with dynamic text (function)
    local toggleEngineUUID = menuManager().addBasicItem({
        text = function() 
            return GetIsVehicleEngineRunning(vehicle) and "Turn Off Engine" or "Turn On Engine" 
        end,
        onRelease = function(item)
            local running = GetIsVehicleEngineRunning(vehicle)
            SetVehicleEngineOn(vehicle, not running, true, false)
        end,
    })
    
    -- Item with dynamic visibility (function)
    local lockDoorsUUID = menuManager().addBasicItem({
        text = "Lock Doors",
        visible = function() return GetVehicleDoorLockStatus(vehicle) == 1 end,
        onRelease = function(item)
            SetVehicleDoorsLocked(vehicle, 2)
        end,
    })
    
    local unlockDoorsUUID = menuManager().addBasicItem({
        text = "Unlock Doors",
        visible = function() return GetVehicleDoorLockStatus(vehicle) ~= 1 end,
        onRelease = function(item)
            SetVehicleDoorsLocked(vehicle, 1)
        end,
    })
    
    -- Status item (non-clickable, just displays info)
    local statusUUID = menuManager().addBasicItem({
        text = function()
            local locked = GetVehicleDoorLockStatus(vehicle) ~= 1
            local engine = GetIsVehicleEngineRunning(vehicle)
            return ("Status: %s | %s"):format(
                engine and "Engine ON" or "Engine OFF",
                locked and "Locked" or "Unlocked"
            )
        end,
        clickable = false,
    })
    
    -- Group all items together - when any item triggers an event,
    -- all items in the group will have their functions re-evaluated
    menuManager().addRefresher({toggleEngineUUID, lockDoorsUUID, unlockDoorsUUID, statusUUID})
end)
```

***

## Vehicle Controls with Refresh Group

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    if not menuType.Vehicle then return end
    
    local vehicle = staticRaycastResult.hitEntity
    local controlsMenu = menuManager().addSubMenu({ text = "Vehicle Controls" })
    
    -- Toggle headlights
    local lightsUUID = menuManager(controlsMenu).addBasicItem({
        text = function() 
            local lightsOn = GetVehicleLightsState(vehicle)
            return lightsOn and "Turn Off Lights" or "Turn On Lights"
        end,
        onRelease = function(item)
            local lightsOn = GetVehicleLightsState(vehicle)
            SetVehicleLights(vehicle, lightsOn and 1 or 3)
        end,
    })
    
    -- Toggle neon (only visible if vehicle has neons)
    local neonUUID = menuManager(controlsMenu).addBasicItem({
        text = function()
            local neonOn = IsVehicleNeonLightEnabled(vehicle, 0)
            return neonOn and "Disable Neons" or "Enable Neons"
        end,
        visible = function() 
            return IsVehicleNeonLightEnabled(vehicle, 0) or IsVehicleNeonLightEnabled(vehicle, 1)
                or IsVehicleNeonLightEnabled(vehicle, 2) or IsVehicleNeonLightEnabled(vehicle, 3)
                or GetVehicleMod(vehicle, 22) ~= -1  -- Has neon mod installed
        end,
        onRelease = function(item)
            local neonOn = IsVehicleNeonLightEnabled(vehicle, 0)
            for i = 0, 3 do
                SetVehicleNeonLightEnabled(vehicle, i, not neonOn)
            end
        end,
    })
    
    -- Vehicle info display
    local infoUUID = menuManager(controlsMenu).addBasicItem({ 
        text = function() 
            local health = GetVehicleEngineHealth(vehicle)
            local speed = GetEntitySpeed(vehicle) * 3.6  -- Convert to km/h
            return ("Health: %.0f | Speed: %.0f km/h"):format(health, speed)
        end, 
        clickable = false 
    })
    
    -- Link items together for partial refresh
    menuManager().addRefresher({lightsUUID, neonUUID, infoUUID})
end)
```

***

## Ped Actions with Refresh Group

```lua
Xtarget.register(function(menuManager, staticRaycastResult, menuType)
    if not menuType.Ped then return end
    if menuType.Player then return end  -- Exclude players
    
    local ped = staticRaycastResult.hitEntity
    
    -- Follow/Stop following toggle
    local followUUID = menuManager().addBasicItem({
        text = function()
            local isFollowing = IsPedInCombat(ped, PlayerPedId())
            return isFollowing and "Stop Following" or "Follow Me"
        end,
        onRelease = function(item)
            local playerPed = PlayerPedId()
            TaskFollowToOffsetOfEntity(ped, playerPed, 0.0, -1.5, 0.0, 5.0, -1, 1.5, true)
        end,
    })
    
    -- Ped status display
    local statusUUID = menuManager().addBasicItem({
        text = function()
            local health = GetEntityHealth(ped) - 100
            local maxHealth = GetEntityMaxHealth(ped) - 100
            local isDead = IsEntityDead(ped)
            if isDead then
                return "Status: Dead"
            end
            return ("Health: %d/%d"):format(health, maxHealth)
        end,
        clickable = false,
    })
    
    -- Heal button (only visible if ped is injured)
    local healUUID = menuManager().addBasicItem({
        text = "Heal Ped",
        visible = function()
            local health = GetEntityHealth(ped)
            local maxHealth = GetEntityMaxHealth(ped)
            return health < maxHealth and not IsEntityDead(ped)
        end,
        onRelease = function(item)
            SetEntityHealth(ped, GetEntityMaxHealth(ped))
        end,
    })
    
    menuManager().addRefresher({followUUID, statusUUID, healUUID})
end)
```