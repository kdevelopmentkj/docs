# Client

## Player State

The script uses a shared player state to indicate when a player is performing a container robbery action.

```lua
LocalPlayer.state.isOnRobberyContainer
```

* **Type**: `boolean`
* **Description**: Indicates whether the player is currently performing a container robbery animation
* **Values**:
  * `true` - Player is actively robbing a container (animation in progress)
  * `false` - Player is not robbing a container

**Usage example:**

```lua
-- Check if a player is currently robbing a container
if LocalPlayer.state.isOnRobberyContainer then
    print("Player is robbing a container")
end

-- For other players (if you have their source/serverId)
local playerState = Player(source).state.isOnRobberyContainer
if playerState then
    print("Player " .. source .. " is robbing a container")
end
```
