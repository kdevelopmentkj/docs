# Server

```lua
local Xgroups = exports.Xgroups:getSharedObject()
```

***

```lua
Xgroups.hasPermission(source, perm)
```

* source: `number`
* perm: `string`
* **return** hasperm: `boolean`

***

```lua
Xgroups.hasBypass(source)
```

* source: `number`
* **return** hasbypass: `boolean`

***

```lua
Xgroups.getPlayerGroups(source)
```

* source: `number`
* **return** groups\_list: `table`

***

```lua
Xgroups.GetPlayerPermissions(source)
```

* source: `number`
* **return** perm\_list: `table`

***

```lua
Xgroups.CreateGroup(name, label)
```

* name: `string`
* label: `string`

***

```lua
Xgroups.DeleteGroup(name)
```

* name: `string`

***

```lua
Xgroups.AddGrade(groupName, gradeName, label)
```

* groupName: `string`
* gradeName: `string`
* label: `string`

***

```lua
Xgroups.RemoveGrade(groupName, gradeName)
```

* groupName: `string`
* gradeName: `string`

***

```lua
Xgroups.AddPermission(groupName, key)
```

* groupName: `string`
* key: `string`

***

```lua
Xgroups.RemovePermission(groupName, key)
```

* groupName: `string`
* key: `string`

***

```lua
Xgroups.AssignPermission(groupName, gradeName, key)
```

* groupName: `string`
* gradeName: `string`
* key: `string`

***

```lua
Xgroups.UnassignPermission(groupName, gradeName, key)
```

* groupName: `string`
* gradeName: `string`
* key: `string`

***

```lua
Xgroups.SetPlayerGroup(source, groupName, gradeName)
```

* source: `number`
* groupName: `string`
* gradeName: `string`

***

```lua
Xgroups.RemovePlayerGroup(source, groupName)
```

* source: `number`
* groupName: `string`

***

```lua
Xgroups.SetBypass(source, bypass)
```

* source: `number`
* bypass: `boolean`
