# Example of use

***

## Pipe an existing script's blips to the custom map

Any resource that already creates GTA blips via the vanilla natives can forward them to JH\_GameUI's custom map with **one manifest line**. No Lua change, no re-write.

**`fxmanifest.lua` of your resource** :

```lua
client_scripts {
    -- 👇 Add this at the top of your client_scripts list
    '@JH_GameUI/blip_wrapper.lua',

    -- your normal client scripts
    'client/main.lua',
}
```

**Your existing blip code keeps working untouched** :

```lua
-- Static blip
local blip = AddBlipForCoord(441.4, -982.1, 30.7)
SetBlipSprite(blip, 60)
SetBlipColour(blip, 3)
SetBlipScale(blip, 0.9)
SetBlipAsShortRange(blip, true)
BeginTextCommandSetBlipName('STRING')
AddTextComponentString('Police Station')
EndTextCommandSetBlipName(blip)

-- Entity-attached blip (vehicle, ped, object)
local vehBlip = AddBlipForEntity(vehicle)
SetBlipSprite(vehBlip, 225)
SetBlipColour(vehBlip, 5)
ShowHeadingIndicatorOnBlip(vehBlip, true)
```

Both blips will appear on the vanilla map (native behaviour is untouched) **and** on the JH\_GameUI custom map. Entity-attached blips are tracked for position + heading automatically.

When your resource stops, every blip it owned is removed from the custom map in one pass — no leaks.

***

## Hide the HUD during a cutscene

```lua
-- Start of cutscene
exports.JH_GameUI:setHudHidden(true)

DoScreenFadeOut(500)
Wait(500)
-- ... camera / animation / dialogue ...
DoScreenFadeIn(500)

-- End of cutscene
exports.JH_GameUI:setHudHidden(false)
```

The minimap, compass, clock, speedometer, player count and every custom blip disappear immediately. The player can still open the pause menu — the canvas editor displays a banner indicating that a script is force-hiding the HUD.
