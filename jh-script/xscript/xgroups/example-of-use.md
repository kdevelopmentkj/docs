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

# Example of use

***

#### Client Side :

```lua
local Xgroups = exports.Xgroups:getSharedObject()

RegisterCommand("spawncar", function(source, args)
    local src = source
    
    if Xgroups:hasPermission("police:vehicle.*") then
        --Success spawn if has perm
    else
        --Not success if not has perm
    end
end)
```

***

#### Server Side :

```lua
local Xgroups = exports.Xgroups:getSharedObject()

Xgroups.CreateGroup("police", "Police")

Xgroups.AddGrade("police", "cadet", "Cadet")
Xgroups.AddGrade("police", "officer", "Officer")
Xgroups.AddGrade("police", "sergeant", "Sergeant")
Xgroups.AddGrade("police", "lieutenant", "Lieutenant")

Xgroups.AddPermission("police", "vehicle.spawn")
Xgroups.AddPermission("police", "vehicle.delete")
Xgroups.AddPermission("police", "arrest")
Xgroups.AddPermission("police", "fine")

Xgroups.AssignPermission("police", "officer", "arrest")
Xgroups.AssignPermission("police", "officer", "fine")
Xgroups.AssignPermission("police", "sergeant", "vehicle.spawn")
Xgroups.AssignPermission("police", "lieutenant", "police:*")

RegisterCommand("spawncar", function(source, args)
    local src = source
    
    if Xgroups:hasPermission(src, "police:vehicle.spawn") then
        --Success spawn if has perm
    else
        --Not success if not has perm
    end
end)
```
