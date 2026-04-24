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

***

## Kick a player with a custom reason from a client script

The custom pause menu already ships a `Quit` button that uses this hook. You can re-use the same state-bag to drop a player from anywhere client-side :

```lua
-- Client side
local function disconnectToLobby()
    LocalPlayer.state:set('dropMe', 'You have been moved to the lobby.', true)
end

RegisterCommand('lobby', disconnectToLobby, false)
```

The JH\_GameUI server script listens to the `dropMe` state-bag change and calls `DropPlayer(src, reason)` with the provided string as the kick reason shown to the player.

***

## Read the online player count

```lua
-- Anywhere, client or server
local online = GlobalState.JH_GameUI_PlayersCounts or 0
local max    = GlobalState.JH_GameUI_maxPlayers or 0

print(('Online: %d / %d'):format(online, max))
```

The values update automatically on `playerJoining` / `playerDropped`. The player-count widget of JH\_GameUI uses the same state bags — you are reading the same source of truth.

***

## Sync a route between passengers of the same vehicle

Nothing to do — it already works. Open your map, set a waypoint while driving : every passenger in your vehicle sees your route drawn on their minimap in purple, with a marker at the destination. When someone changes seat or leaves the vehicle, the waypoint clears from their map automatically.

If you want to **read** another passenger's current waypoint from your own script :

```lua
-- Client side — look at passenger whose server id is `sid`
local wp = Player(sid).state['JH_GameUI:wp']
if wp and wp.vehNetId == NetworkGetNetworkIdFromEntity(GetVehiclePedIsIn(PlayerPedId(), false)) then
    print(('[%s] waypoint: %.1f, %.1f (seat %d)'):format(wp.name, wp.x, wp.y, wp.seat))
end
```

The key follows `config.storage.kvpPrefix` — default is `JH_GameUI:wp`.

***

## Force a blip wrapper resync after a manual reload

Rarely needed — the wrapper auto-resyncs on boot and whenever a new resource starts. But if you have just restored blips from a snapshot file and want them pushed immediately :

```lua
-- From within the resource that owns those blips
exports[GetCurrentResourceName()]:__JH_GameUI_blipWrapperResync()
-- or, equivalent :
TriggerEvent('JH_GameUI:blips:wrapperResync')
```
