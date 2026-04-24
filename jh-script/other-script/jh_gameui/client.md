# Client

JH\_GameUI exposes a minimal, stable public API. Everything else is internal plumbing between Lua and the NUI bundle.

***

## Blip wrapper integration

The primary integration point. Other resources do **not** need to know about any JH\_GameUI internal — they keep calling the vanilla natives (`AddBlipForCoord`, `SetBlipSprite`, `SetBlipColour`, …) and JH\_GameUI transparently mirrors every blip to its custom map.

To enable the wrapper in another resource, add **one line** to that resource's `fxmanifest.lua` :

```lua
client_scripts {
    '@JH_GameUI/blip_wrapper.lua',
    -- your own client scripts after
    'client/main.lua',
}
```

* ✅ All `AddBlipFor*` variants are captured : `AddBlipForCoord`, `AddBlipForEntity`, `AddBlipForArea`, `AddBlipForRadius`, `AddBlipForPickup`.
* ✅ All `SetBlip*` setters are captured : sprite, colour, alpha, scale, rotation, shortRange, flashes, display, priority, category, highDetail, hiddenOnLegend, friendly, bright, shrink, showCone, showNumber, showTick, showHeading, showCrew, showFriend, showOutline, route, routeColour, secondary colour, scale transformation, fade.
* ✅ Blip names are captured (`BeginTextCommandSetBlipName` / `AddTextComponentString` / `EndTextCommandSetBlipName`, `SetBlipNameFromTextFile`, `SetBlipNameToPlayerName`).
* ✅ Entity-attached blips are polled at `500ms` with a 0.3m move threshold and a 2° heading threshold — no duplicate push if nothing moved.
* ✅ `RemoveBlip` and `ClearAllBlipRoutes` are captured. On resource stop, every blip owned by that resource is removed from the custom map automatically.
* ✅ Resources can start / stop in any order. The wrapper re-syncs itself when JH\_GameUI reaches the `started` state or when a new JH\_GameUI version is live-restarted.

***

```lua
exports[yourResource]:__JH_GameUI_blipWrapperResync()
```

_(manual resync — rarely needed)_

* **return** void
* Forces an immediate resync of every blip owned by the **calling** resource. Useful if you have manually restored blips from a snapshot and want them pushed to the custom map right now instead of waiting for the next native call.
* You can also fire the event form :
  ```lua
  TriggerEvent('JH_GameUI:blips:wrapperResync')
  ```

***

## HUD visibility

```lua
exports.JH_GameUI:setHudHidden(hidden)
```

* hidden: `boolean` — `true` hides every widget of the HUD (minimap, compass, clock, speedometer, players-count, blips…). `false` shows them again.
* **return** void

Useful for cinematics, cutscenes, death animations, driving tests, admin blackouts… The canvas editor remains available in the pause menu and displays a small banner reminding the player that the HUD is currently force-hidden.

The state is authoritative client-side — the NUI re-syncs the flag on mount, so reloading the NUI mid-session keeps the correct visibility.

***

## Server-side : disconnect with reason

The custom pause menu's `Quit` button writes a reason into a player state-bag that the bundled `server/main.lua` consumes to call `DropPlayer(src, reason)`. You can hook the same mechanism from your own scripts :

```lua
-- client side, from any resource
LocalPlayer.state:set('dropMe', 'You have been moved to the lobby.', true)
```

The server will `DropPlayer` that player with the provided reason string.

***

## State bags

JH\_GameUI maintains the following state bags (read-only from other scripts) :

* `GlobalState.JH_GameUI_PlayersCounts` : `number` — current online player count.
* `GlobalState.JH_GameUI_maxPlayers` : `number` — `sv_maxClients` convar value.
* `Player(sid).state['JH_GameUI:wp']` : `table` or `nil` — current waypoint of that player **if they are in a vehicle**. Shape :
  * vehNetId: `number` — network id of the vehicle they are in
  * x, y: `number` — waypoint coords
  * name: `string` — player name
  * trajId: `number` — incrementing counter per re-plot
  * seat: `number` — seat index (-1 = driver)
  * route: `table` — sampled route nodes `{x, y, z}` (may be `nil` while the route is still being computed)

The `JH_GameUI:` prefix in the waypoint key matches `config.storage.kvpPrefix` — if you change that prefix, this state-bag key changes too.

***

## Net events (internal)

The events below are emitted by JH\_GameUI itself and documented for completeness. They are **not** part of the integration surface — treat them as internal.

* `JH_GameUI:share:invite` _(C→S)_ — NUI-initiated custom-blip share request.
* `JH_GameUI:share:respond` _(C→S)_ — Target's accept/deny.
* `JH_GameUI:share:inviteIncoming` _(S→C)_ — Target sees the modal.
* `JH_GameUI:share:inviteSent` _(S→C)_ — Sender confirmation.
* `JH_GameUI:share:inviteRejected` _(S→C)_ — Rate-limit / distance / pending-cap rejection.
* `JH_GameUI:share:result` _(S→C)_ — Final accept / deny / peer-left notification.
* `JH_GameUI:blips:wrapperResync` _(local)_ — Triggers a manual blip-wrapper resync.
