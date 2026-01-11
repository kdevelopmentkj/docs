# Server

The editable file is located at `server/sv_editable.lua` and can be modified directly.

***

**Available functions :**

```lua
canOpenContainer(src)
```

* **src**: `number` - Source (player ID)
* **return**: `boolean` - Whether the player can open the container
* Called when a player tries to open a container
* You can check if police player count is greater than 0, check time restrictions, or any other condition
* Default: `return true`

***

```lua
onNoRewards(src)
```

* **src**: `number` - Source (player ID)
* **return**: `boolean` - Whether the function executed successfully
* Called when a player tries to get rewards from an empty container or during cooldown
* You can send a message to the player
* Default: `return true`

***

```lua
onRewards(src)
```

* **src**: `number` - Source (player ID)
* **return**: `boolean` - Whether the function executed successfully
* Called when a player successfully opens a container and should receive rewards
* You can give rewards to the player (items, money, etc.)
* Default: `return true`
