---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Client

```lua
local Xgroups = exports.Xgroups:getSharedObject()
```

***

```lua
Xgroups.hasGlobal(options)
```

* options: `table`
  * groupName: `string` (optionnel) AUTO Check hasGroup() if not gradeName
  * gradeName: `string` (optionnel) AUTO Check hasGradeByGroup() if gName and gName
  * permission: **perm** `string` (optionnel) AUTO Check hasPermission()
  * duty: **groupName** `string` (optionnel) AUTO Check isDuty()
  * bypass: `boolean` (optionnel) AUTO Check hasBypass()
* **return** hasaccess: `boolean`

***

```lua
Xgroups.hasPermission(perm)
```

* perm: `string`
* **return** hasperm: `boolean`

***

```lua
Xgroups.hasGroup(groupName)
```

* groupName: `string`
* **return** hasgroup: `boolean`

***

```lua
Xgroups.hasGradeByGroup(groupName, gradeName)
```

* groupName: `string`
* gradeName: `string`
* **return** hasgroup: `boolean`

***

```lua
Xgroups.hasBypass()
```

* **return** hasbypass: `boolean`

***

```lua
Xgroups.isDuty()
```

* **return** isduty: `boolean`

***

```lua
Xgroups.setDuty(groupName, duty)
```

* groupName: `string`
* duty: `boolean`
* **return** edited: `boolean`

***

```lua
Xgroups.getPlayerGroups()
```

* **return** groups\_list: `table`

***

```lua
Xgroups.getPlayerCompatibilityGroups()
```

* **return** compatibility\_jobs\_list: `table`

***

```lua
Xgroups.getPlayerPermissions()
```

* **return** perm\_list: `table`

***

```lua
Xgroups.getPlayerDuty()
```

* **return** duty\_list: `table`

***

```lua
Xgroups.getPlayerCompatibilityDuty()
```

* **return** compatibility\_duty\_list: `table`
