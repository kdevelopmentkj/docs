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
Xgroups.hasPermission(perm)
```

* perm: `string`
* **return** hasperm: `boolean`

***

```lua
Xgroups.hasBypass()
```

* **return** hasbypass: `boolean`

***

```lua
Xgroups.getPlayerGroups()
```

* **return** groups\_list: `table`

***

```lua
Xgroups.getPlayerPermissions()
```

* **return** perm\_list: `table`
