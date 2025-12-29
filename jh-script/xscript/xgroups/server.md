# Server

```lua
local Xgroups = exports.Xgroups:getSharedObject()
```

***

```lua
Xgroups.hasGlobal(source, options)
```

* source: `number`
* options: <kbd>table</kbd>
  * groupName: <kbd>string</kbd> (optionnel) AUTO Check hasGroup()
  * permission: **perm** <kbd>string</kbd> (optionnel) AUTO Check hasPermission()
  * duty: **groupName** <kbd>string</kbd> (optionnel) AUTO Check isDuty()
  * bypass: <kbd>boolean</kbd> (optionnel) AUTO Check hasBypass()
* **return** hasaccess: `boolean`

***

```lua
Xgroups.hasPermission(source, perm)
```

* source: `number`
* perm: `string`
* **return** hasperm: `boolean`

***

```lua
Xgroups.hasGroup(source, groupName)
```

* source: `number`
* groupName: `string`
* **return** hasgroup: `boolean`

***

```lua
Xgroups.hasBypass(source)
```

* source: `number`
* **return** hasbypass: `boolean`

***

```lua
Xgroups.isDuty(source)
```

* source: `number`
* **return** isduty: `boolean`

***

```lua
Xgroups.setDuty(source, groupName, duty)
```

* source: `number`&#x20;
* groupName: `number`&#x20;
* duty: `boolean`
* **return** edited: `boolean`

***

```lua
Xgroups.getPlayerGroups(source)
```

* source: `number`
* **return** groups\_list: `table`&#x20;

***

```lua
Xgroups.getPlayerCompatibilityGroups(source)
```

* source: `number`
* **return** compatibility\_jobs\_list: `table`

***

```lua
Xgroups.getPlayerPermissions(source)
```

* source: `number`
* **return** perm\_list: `table`

***

```lua
Xgroups.getPlayerDuty(source)
```

* source: `number`
* **return** duty\_list: `table`

***

```lua
Xgroups.getPlayerCompatibilityDuty(source)
```

* source: `number`
* **return** compatibility\_duty\_list: `table`

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
