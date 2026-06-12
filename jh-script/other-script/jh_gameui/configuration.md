# Configuration

The configuration file is located at `config.lua` (in the resource root) and is the **only** Lua file accessible after escrow (`escrow_ignore`). Every option here is read at resource start.

All values below live under the shared global `Minimap.config`.

***

**Available options :**

* **`keybinds`** : `table`
  * **`bigmap`** : `table` — **press-hold** keybind. While held, the minimap flips to the bigmap.
    * key: `string` — Keyboard key (default: `'Z'`)
    * label: `string` — Label shown in the FiveM keybind UI (default: `'Minimap Big'`)
  * **`bigmapToggle`** : `table` — **toggle** keybind. Each press flips the bigmap on/off.
    * key: `string` — Defaults to empty (no key) to avoid conflicting with `bigmap`. The player is expected to rebind it manually from the FiveM settings.
    * label: `string` — (default: `'Minimap Big (Toggle)'`)
  * **`pauseMenuControls`** : `table` — GTA native control IDs that open the custom pause menu.
    * Default: `{199, 200}` (`FRONTEND_PAUSE` + `FRONTEND_PAUSE_ALTERNATE` / ESC).
  * **`pauseMenuCooldownMs`** : `number` — Anti-spam : minimum delay (ms) before the pause menu can re-open after an open **or** a close. Stops hammering ESC/P from spamming the open/close (default: `400`).

***

* **`polling`** : `table` — Per-stream polling cadences. The stream pattern kills the coroutine as soon as no NUI subscriber holds the stream → no idle cost outside subscribers.
  * **`pos`** : `table` — Player coords + heading. Consumed by map widget, waypoint progress, blip sidebar distance, and custom-blip share distance check.
    * active: `number` — Delay (ms) after a push that actually changed something (default: `100`)
    * idle: `number` — Delay (ms) after a poll where nothing changed (default: `250`)
  * **`cam`** : `table` — `GetFinalRenderedCamRot(2)`. Consumed by the compass and by the map widget when `rotateWithCam` is ON.
    * active: `number` (default: `100`)
    * idle: `number` (default: `250`)
    * pauseIdle: `number` — Cadence used while the pause menu is open (map owns its own bearing in this state, polling is skipped) (default: `500`)
  * **`stats`** : `table` — HP + armor.
    * active: `number` (default: `1500`)
  * **`vehicle`** : `table` — Speedometer thread. Only exists **while** the player is in a vehicle (entry detected via `CEventNetworkPlayerEnteredVehicle`, exit detected by a lightweight watchdog). Covers speed, rpm, gear, fuel, engine health.
    * active: `number` (default: `100`)
    * idle: `number` (default: `250`)
  * **`street`** : `table` — Current street name thread (`GetStreetNameAtCoord`). Pushes only when the street hash changes ; idle backs off while the player stays on the same road. Sub-gated : the thread dies when the street widget is disabled.
    * active: `number` (default: `100`)
    * idle: `number` (default: `250`)
  * **`voice`** : `table` — Local talking indicator (`MumbleIsPlayerTalking`, the same check pma-voice uses). Drives the mic icon ; proximity / radio / call changes are event-driven (state bags), not polled. The whole Voice module no-ops when pma-voice isn't running.
    * active: `number` (default: `100`)
    * idle: `number` (default: `250`)

***

* **`sync`** : `table`
  * **`nuiBatchMs`** : `number` — Coalescing window for state-bag changes before pushing to NUI. "Last value wins" within this window → turns N rapid mutations into a single push (default: `50`)
  * **`exitWatchMs`** : `number` — Polling cadence for vehicle-exit detection (no reliable native event exists for the exit side of a vehicle) (default: `1500`)

***

* **`roads`** : `table` — GPS route sampling (used to draw the waypoint route on the minimap).
  * **`sampleStep`** : `number` — Distance in meters between two sampled points along a route. Smaller = smoother rendering, more expensive to compute (default: `6.0`)
  * **`gpsWaitMaxMs`** : `number` — Timeout before giving up when waiting for the GPS route to build (default: `5000`)
  * **`yieldEveryIter`** : `number` — CPU yield (`Wait(0)`) every N iterations of sampling. Prevents long stalls on very long routes (default: `100`)
  * **`gpsSlot`** : `number` — GPS slot to use. GTA exposes slots `0` and `1`. We use slot `1` by default (less commonly used by other scripts) (default: `1`)

***

* **`wrapper`** : `table`
  * **`resyncDelayMs`** : `number` — Delay (ms) before the global blip-wrapper resync wake-up at boot. Gives other resources time to reach the `started` state before we poke them (default: `1000`)

***

* **`storage`** : `table`
  * **`kvpPrefix`** : `string` — Prefix used for every client-side KVP key written by JH\_GameUI (pause-menu settings, custom blips, …) (default: `'JH_GameUI:'`)
  * ⚠️ Changing this prefix breaks existing KVP data on clients. Only touch it for multi-instance / save-slot setups.

***

* **`enforce`** : `table` — Cannot be overridden by the player (no corresponding toggle in the canvas editor).
  * **`minimapOnlyInVehicle`** : `boolean`
    * `true` — Minimap is only rendered while the player is in a vehicle. Uses the same `vehicle:state` detection as the speedometer widget.
    * `false` — Minimap always visible (default: `false`)
  * **`bigmapOnlyInVehicle`** : `boolean`
    * `true` — The pause-menu big map is only available in a vehicle. On foot it is visually disabled (placeholder) and its map tools are hidden. Does **not** affect the bigmap keybinds (`bigmap` / `bigmapToggle`).
    * `false` — Pause big map always available (default: `false`)

***

* **`share`** : `table` — Rules for the custom-blips share feature (players sending blip lists to nearby players).
  * **`maxDistance`** : `number` — Max distance (meters) between sender and target for a share to be authorized. The server checks this with authoritative `GetEntityCoords` — clients cannot spoof their position (default: `5.0`)
  * **`rateLimitMs`** : `number` — Minimum time (ms) between two successive invites from the same sender. Anti-spam safeguard (default: `5000`)
  * **`maxBlipsPerInvite`** : `number` — Max blips per invite. Protects against oversized payloads (default: `50`)
  * **`maxPendingPerSender`** : `number` — Max simultaneous pending invites **from** the same sender. Prevents burst-spamming multiple targets during the `rateLimitMs` window by cycling through them (default: `3`)
  * **`maxPendingPerTarget`** : `number` — Max simultaneous pending invites **to** the same target. Anti collective-harassment : N senders can't pile modals on one player (default: `5`)

***

* **`contextMenu`** : `table` — Right-click context menu shown on the bigmap.
  * **`buttons`** : `table` — Array of button definitions. Each entry :
    * **`id`** : `string` _(required)_ — Unique key. On duplicate ids, the first one wins.
    * **`label`** : `string` — Text shown in the menu entry.
    * **`icon`** : `string` — [Lucide](https://lucide.dev/icons) icon name (e.g. `'map-pin'`). Unknown / missing name falls back to `'circle'`.
    * **`order`** : `number` — Sort weight, ascending (optional, default: `0`).
    * **`canAccess`** : `function(ctx) -> boolean` — Visibility predicate (optional). Missing = always visible. Crash = hidden.
    * **`action`** : `function(ctx)` — Run on click (optional). Re-authorized via `canAccess` before running.
    * The `ctx` passed to both callbacks is `{ startPos = { x, y, z }, endPos = { x, y } }` — `startPos` is the player's current position, `endPos` is the clicked map point (2D ; sample the ground Z yourself if needed).

***

* **`notify`** : `table` — Defaults for the notification system (look / position / stacking are NUI-side and player-editable).
  * **`defaultType`** : `string` — Type used when the caller omits it : `'info'` | `'success'` | `'warning'` | `'error'` (default: `'info'`)
  * **`defaultDuration`** : `number` — Fallback duration (ms) when neither `duration` nor a per-type duration applies (default: `4000`)
  * **`durations`** : `table` — Per-type default duration (ms) when the caller omits `duration`.
    * `info` (default: `4000`), `success` (default: `3500`), `warning` (default: `5000`), `error` (default: `6000`)

***

* **`chat`** : `table` — Drop-in replacement for the stock `chat` resource. Remember to **stop the stock `chat`** (do not `ensure chat`) — running both double-renders messages.
  * **`enableGlobalMessages`** : `boolean` — How plain text (non-command) messages are handled.
    * `true` — Relayed to the server, which re-broadcasts to **all** players (authoritative ; the sender only sees their own message once it comes back). Standard public chat (default).
    * `false` — **No** server trigger : the message stays **local** to the player and is shown as `Privé`. For servers that don't want a public chat. Commands (`/…`) and the `/staff` channel still work.
  * **`enableJoinMessages`** : `boolean` — Announce `X joined the server.` in every player's chat on connect (default: `true`).
  * **`enableQuitMessages`** : `boolean` — Announce `X left the server.` in every player's chat on disconnect (default: `true`).
  * **`defaultColor`** : `table` — Default `{ r, g, b }` color of global messages (default: `{ 255, 255, 255 }`).
  * **`guardColor`** : `table` — `{ r, g, b }` color of **system** messages tagged `GUARD` (unknown command, "not allowed" and other server notices). These are personal notices and are never stored in history (default: `{ 245, 158, 11 }`).
  * **`openKey`** : `string` — Key that opens the chat input. Registered via `RegisterKeyMapping`, so it is **rebindable in-game** from the FiveM key settings (default: `'T'`).
  * **`historySize`** : `number` — Number of recent messages the **server** keeps and restores to a player when their chat (re)loads. History is filtered by what each player may see : global messages to everyone, channel messages only to players holding the channel's `seObject` ACE. Private messages (`enableGlobalMessages = false`) are never stored server-side. `0` disables history (default: `100`).

> Custom chat **modes / channels** (for example a staff channel) are **not** configured here — they are registered at runtime from any resource with `exports['JH_GameUI']:registerMode({...})`, optionally gated by an ACE via `seObject`, and you can intercept every message with `exports['JH_GameUI']:registerMessageHook(fn)`. `config.lua` ships a commented staff-channel example at its bottom (server-only branch). See [Example of use](example-of-use.md).

> The chat's look (position, size, opacity, background style, text scale, line spacing, timestamps, idle fade, max messages, lock-to-input, message grouping, search) is **NUI-side and player-editable** from the canvas editor — none of it lives in `config.lua`. The two editable widgets are **Chat** (the message log) and **Chat input** (the typing bar).

***

* **`voice`** : `table` — Voice HUD indicator. An **add-on** to [pma-voice](https://github.com/AvarianKnight/pma-voice) — the whole module no-ops when pma-voice isn't running. The widget reads pma's state bags (proximity mode, radio channel / talk, call channel) and the local mic state ; it never changes how voice works.
  * **`resource`** : `string` — pma-voice resource name, used to detect it is running and as the convar owner. A renamed fork only needs this changed (default: `'pma-voice'`).
  * **`disablePmaUi`** : `boolean` — `true` makes the server set the replicated convar `voice_enableUi = 0` so pma-voice's **own** talk overlay is hidden and only JH\_GameUI's voice widget shows. `false` leaves pma-voice's UI as configured in your `server.cfg` (default: `true`).

***

* **`debug`** : `table`
  * **`failFocusCommand`** : `string` — Console command (F8) that force-recovers NUI focus if the pause menu ever desyncs and traps the cursor. Run `/gameui_failFocus` (default: `'gameui_failFocus'`)
  * **`wrapper_debug`** : `boolean` — Verbose logging for the native blip-wrapper pipeline (blip interception → NUI sync). Noisy — keep `false` in production (default: `false`)

***

* **Extra status bars (hunger & thirst)** — Defined at the bottom of `config.lua` (client only), **not** under `Minimap.config`. The minimap can show two extra bars : hunger (left, yellow) and thirst (right, blue), driven by the global function :
  * **`syncExtraStatus(hunger, thirst)`** — `hunger` / `thirst` are numbers `0`–`100` (clamped, rounded). Deliberately **not** an export : only `config.lua` (same resource) can call it. The first call activates the bars and adds their on/off toggle to the minimap edit cog. Wire it to your framework's hunger/thirst system — `config.lua` ships commented examples for ESX (`esx_status:onTick`) and a generic poll.
