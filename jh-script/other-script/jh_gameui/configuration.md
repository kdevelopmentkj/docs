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

* **`enforce`** : `table` — Server-side enforced policies. Cannot be overridden by the player (no corresponding toggle in the canvas editor).
  * **`minimapOnlyInVehicle`** : `boolean`
    * `true` — Minimap is only rendered while the player is in a vehicle. Uses the same `vehicle:state` detection as the speedometer widget.
    * `false` — Minimap always visible (default: `false`)

***

* **`share`** : `table` — Rules for the custom-blips share feature (players sending blip lists to nearby players).
  * **`maxDistance`** : `number` — Max distance (meters) between sender and target for a share to be authorized. The server checks this with authoritative `GetEntityCoords` — clients cannot spoof their position (default: `5.0`)
  * **`rateLimitMs`** : `number` — Minimum time (ms) between two successive invites from the same sender. Anti-spam safeguard (default: `5000`)
  * **`maxBlipsPerInvite`** : `number` — Max blips per invite. Protects against oversized payloads (default: `50`)
  * **`maxPendingPerSender`** : `number` — Max simultaneous pending invites **from** the same sender. Prevents burst-spamming multiple targets during the `rateLimitMs` window by cycling through them (default: `3`)
  * **`maxPendingPerTarget`** : `number` — Max simultaneous pending invites **to** the same target. Anti collective-harassment : N senders can't pile modals on one player (default: `5`)
