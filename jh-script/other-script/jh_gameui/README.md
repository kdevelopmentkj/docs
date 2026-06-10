# JH\_GameUI

{% embed url="https://kdev-jh.tebex.io/package/jh-gameui" %}

**JH\_GameUI** is a complete replacement for GTA V's native HUD on FiveM, rebuilding the minimap, bigmap, pause menu, speedometer, compass, clock, chat and blip rendering in a modern NUI. The game-native radar is disabled and every widget is streamed from Lua through a subscription pipeline, so widgets that are not displayed do not cost CPU. A drag-and-drop **canvas editor** lets the player reposition, resize and reskin each widget live ; a **custom blips editor** lets the player create / share / import map markers during gameplay. A transparent **blip wrapper** captures every native `AddBlipFor*` / `SetBlip*` call from any other resource and renders it on the custom map without code change. A drop-in **chat** replaces the stock `chat` resource — the same `chat:addMessage` / `chat:addSuggestion` API, so ESX, ox\_lib and every existing resource keep working unchanged.

> _"Stop drawing on the native minimap, replace it. JH\_GameUI : a full HUD you can configure from inside the game."_

#### ✨ Key Features

* **Full native HUD replacement** — the game radar is disabled ; minimap, bigmap, pause menu, compass, speedometer, clock, player-count, chat and blip rendering are all redrawn from scratch.
* **Drop-in chat** — a full replacement for the stock `chat` resource that re-exposes the exact same API (`chat:addMessage`, `chat:addSuggestion(s)`, `chat:removeSuggestion`, the `chatMessage` / `_chat:messageEntered` events, `exports.JH_GameUI:addMessage`). Existing resources (ESX, ox\_lib, custom commands) keep working with **zero code change** — just stop the stock `chat`. Command autocomplete is seeded from the client's `GetRegisteredCommands` **and** the server's ACE-filtered command list. Includes command history (↑/↓), `/command` execution, a built-in server-side `/staff` channel and config-gated join/quit announcements.
* **In-game canvas editor** — every widget (position, size, scale, colors, border mode, visibility) is editable live by the player from the pause menu. Settings are persisted per-player in local KVP storage.
* **Custom blips editor** — players can place, rename, style and remove personal map markers at runtime. Blips are persisted in KVP.
* **Custom blips sharing** — proximity-based (`5m` by default), server-authoritative invite / accept flow with per-sender and per-target rate limits.
* **Transparent blip wrapper** — any other resource simply loads `@JH_GameUI/blip_wrapper.lua` in its `client_scripts` and every `AddBlipFor*` / `SetBlip*` call is automatically mirrored to the custom map. Entity-attached blips are tracked (position + heading) with a diff filter.
* **Waypoint vehicle sync** — passengers of the same vehicle see each other's GPS route drawn on the minimap (state-bag based, batched).
* **Pause menu** — a custom pause overlay replaces ESC. Pass-through to the native settings is available via the `Game Settings` button.
* **Custom pause-menu disconnect** — a `Quit` button that disconnects the player with a developer-supplied reason (driven by a state-bag hook server-side).
* **Bigmap bindings** — press-hold (`Z` by default) AND optional toggle key, with `enforce.minimapOnlyInVehicle` / `enforce.bigmapOnlyInVehicle` admin policies.
* **Notifications** — a styled NUI notification system. Trigger from any resource via `exports.JH_GameUI:notify({...})` (client) or `TriggerClientEvent('JH_GameUI:notify', src, {...})` (server). Look, position, opacity, scale and stacking are player-editable from the canvas editor.
* **Map context menu** — right-click the bigmap to open a developer-defined context menu (teleport, set GPS, …). Each button declares  `canAccess(ctx)` / `action(ctx)` callbacks ; the action is re-authorized on click (client cannot spoof it).
* **Extra status bars** — optional hunger & thirst bars on the minimap, driven by `syncExtraStatus(hunger, thirst)`. Framework-agnostic (ESX / generic poll), wired from `config.lua`. OFF until the first call ; the player can hide them from the minimap edit cog.
* **HUD force-hide** — `exports.JH_GameUI:setHudHidden(true)` lets any other resource hide the entire HUD (e.g. for cinematics, death scenes, driving tests).

#### 🧩 Integration surface

JH\_GameUI is designed to plug into an existing server without code changes :

1. Start JH\_GameUI anywhere in your `server.cfg`.
2. **Stop / remove the stock `chat` resource** (do **not** `ensure chat`). JH\_GameUI ships a drop-in chat that re-exposes the same API ; running both at once double-renders messages. If you don't want JH\_GameUI's chat, disable its widget from the canvas editor instead.
3. _(Optional)_ In each resource that creates blips, add `client_script '@JH_GameUI/blip_wrapper.lua'` to its `fxmanifest.lua` — their blips will now appear on the custom map.
4. _(Optional)_ Call `exports.JH_GameUI:setHudHidden(true/false)` from your cinematic / cutscene scripts.

Everything else is configured in-game by each player from the pause menu.

***

**Dependence :**

* None — standalone system.
