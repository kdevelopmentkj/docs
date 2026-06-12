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

## Send a notification

From **any client** resource via the export :

```lua
exports.JH_GameUI:notify({
    title       = 'Garage',                 -- optional
    description = 'Your vehicle is stored.', -- required (or title)
    type        = 'success',                -- 'info' | 'success' | 'warning' | 'error'
    duration    = 4000,                      -- optional, ms (else per-type default)
    icon        = 'car',                     -- optional, Lucide name (overrides type icon)
    iconColor   = '#22c55e',                 -- optional
    id          = 'garage_store',            -- optional, replaces a notification with the same id
})
```

From the **server**, target a player with the event :

```lua
TriggerClientEvent('JH_GameUI:notify', src, {
    description = 'You received $500.',
    type        = 'success',
})
```

At minimum, pass `title` **or** `description` — an empty notification is ignored. The visual style, position, opacity, scale and stacking are owned by the NUI and edited by the player from the canvas editor.

***

## Add a button to the map context menu

Context-menu buttons are declared in `config.lua` under `Minimap.config.contextMenu.buttons`. They run callbacks — `action` is only executed after `canAccess` passes again on click, so a client cannot spoof it.

```lua
Minimap.config.contextMenu.buttons = {
    {
        id    = 'tp_here',
        label = 'Teleport here',
        icon  = 'map-pin',  -- https://lucide.dev/icons
        order = 10,
        canAccess = function(ctx)
            -- e.g. restrict to admins:  return IsPlayerAdmin()
            return true
        end,
        action = function(ctx)
            -- endPos is 2D — sample the ground Z for the teleport:
            local found, z = GetGroundZFor_3dCoord(ctx.endPos.x, ctx.endPos.y, 1000.0, false)
            SetEntityCoords(PlayerPedId(), ctx.endPos.x, ctx.endPos.y, found and z or 100.0, false, false, false, false)
        end,
    },
}
```

`ctx` is `{ startPos = { x, y, z }, endPos = { x, y } }` — `startPos` is the player's current position, `endPos` is the clicked point on the map.

***

## Show hunger & thirst bars

JH\_GameUI can render two extra bars (hunger + thirst) on the minimap. They stay hidden until the global `syncExtraStatus(hunger, thirst)` function is called from `config.lua` (it is intentionally **not** an export — only the resource's own config can switch the bars on).

**ESX (`esx_status`)** — wire it in `config.lua` :

```lua
AddEventHandler('esx_status:onTick', function(data)
    local hunger, thirst = 100, 100
    for _, s in ipairs(data) do
        if     s.name == 'hunger' then hunger = s.percent
        elseif s.name == 'thirst' then thirst = s.percent end
    end
    syncExtraStatus(hunger, thirst)
end)
```

**Generic poll (any framework / statebag / export)** :

```lua
CreateThread(function()
    while true do
        Wait(1000)
        local hunger = LocalPlayer.state.hunger or 100   -- replace source
        local thirst = LocalPlayer.state.thirst or 100   -- replace source
        syncExtraStatus(hunger, thirst)
    end
end)
```

Values are clamped to `0`–`100` and rounded. The first call activates the bars and adds their on/off toggle to the minimap edit cog — the player can hide them from there.

***

## Use the chat (drop-in for the stock `chat`)

JH\_GameUI ships a full replacement for the FiveM `chat` resource and re-exposes the **exact same API**. **Stop the stock `chat`** (do not `ensure chat`) — then every existing resource keeps working with no change.

**Print a message** — any of the stock forms work, client **or** server :

```lua
-- Server → everyone
TriggerClientEvent('chat:addMessage', -1, {
    color = { 0, 153, 255 },
    multiline = true,
    args = { 'Dispatch', 'A robbery is in progress downtown.' },
})

-- Server → one player (src)
TriggerClientEvent('chat:addMessage', src, {
    args = { 'System', 'Welcome back!' },
})

-- Client (local message, this player only)
exports.JH_GameUI:addMessage({ args = { 'Garage', 'Vehicle stored.' } })
-- or the stock event form:
TriggerEvent('chat:addMessage', { args = { 'Garage', 'Vehicle stored.' } })
```

A message is `{ color = {r,g,b}, multiline = true, args = { author, text } }`. With a single `args` entry it renders as a system line (no author). Plain strings and the deprecated `chatMessage` (`author, color, text`) form are also accepted for compatibility.

**Register command autocomplete** — same stock events ; suggestions show as the player types `/` :

```lua
TriggerEvent('chat:addSuggestion', '/car', 'Spawn a vehicle', {
    { name = 'model', help = 'Vehicle model name' },
})
-- remove it later:
TriggerEvent('chat:removeSuggestion', '/car')
```

You usually don't need to do this for plain commands : JH\_GameUI **auto-seeds** the autocomplete from the client's `GetRegisteredCommands` and from the server's ACE-filtered command list (re-sent whenever the chat NUI reloads, e.g. on reconnect). `chat:addSuggestion` is only needed to add per-argument help.

**Clear the chat** :

```lua
TriggerEvent('chat:clear')          -- or TriggerClientEvent('chat:clear', src)
```

***

## Register a chat mode / channel (e.g. a staff channel)

JH\_GameUI's chat is hook-compatible with the stock FiveM `chat` resource. From **any resource, server-side**, register a **mode** (a channel the player selects in the chat) and gate it behind an ACE with `seObject`. The channel then only appears in the selector for allowed players, only they can send in it, and the server-side history is filtered the same way — all automatic.

```lua
-- server side, after JH_GameUI has started
exports['JH_GameUI']:registerMode({
    name        = 'staff',
    displayName = 'Staff',
    color       = '#e95420',
    isChannel   = true,
    seObject    = 'jh.staff',          -- ACE the player must hold to see / use it
    cb = function(source, message, hook)
        hook.setSeObject('jh.staff')   -- deliver this message to ACE holders only
    end,
})
```

The `cb(source, message, hook)` receives the sender, the outgoing message, and a `hook` :

* `hook.cancel()` — drop the message entirely.
* `hook.setRouting(targets)` — send only to a list of player server ids.
* `hook.setSeObject(ace)` — send only to players holding that ACE.
* `hook.updateMessage(t)` — patch fields of the outgoing message (`color`, `args`, `template`, `params`, …).

Without `seObject` the channel is visible to everyone in the selector, and your `cb` decides who may actually send / receive — for a framework admin check instead of an ACE :

```lua
local function isStaff(src)
    -- return ESX.GetPlayerFromId(src) ~= nil and ESX.GetPlayerFromId(src).getGroup() ~= 'user'
    return false
end

exports['JH_GameUI']:registerMode({
    name = 'staff', displayName = 'Staff', color = '#e95420', isChannel = true,
    cb = function(source, message, hook)
        if not isStaff(source) then hook.cancel(); return end
        local staff = {}
        for _, p in ipairs(GetPlayers()) do
            if isStaff(tonumber(p)) then staff[#staff + 1] = p end
        end
        hook.setRouting(staff)
    end,
})
```

A ready-to-uncomment version of both approaches ships at the bottom of `config.lua` (server-only branch). Modes and hooks registered by a resource are removed automatically when that resource stops.

***

## Hook every chat message

`registerMessageHook` runs your function over **every** message before it is routed — filter profanity, prefix authors, recolor, or cancel :

```lua
-- server side
exports['JH_GameUI']:registerMessageHook(function(source, message, hook)
    -- message.args = { author, text }  (or { text } for a system line)
    local body = type(message.args) == 'table' and message.args[#message.args] or ''
    if body:find('badword') then
        hook.cancel()
    end
end)
```
