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

_**This is just one example !**_\
_**In itself, Xstates works exactly like Fivem's States bag! So you can do exactly the same thing, but with simplifications and optimization.**_

***

#### To Import on fxmanifest.lua :

```lua
shared_script '@Xstates/imports.lua' 
```

#### Shared Side :

```lua
-- GlobalState Example :
Xstates.GlobalState['Mystate'] = {
    data = 'data Global',
    table = {
        data2 = 'data2 Global',
    },
}
Xstates.GlobalState['Mystate'].data = 'New data Global'
Xstates.GlobalState['Mystate'].table.data2 = 'New data2 Global'

-- Entity State Example :
Xstates.Entity(1).state.Mystate = {
    data = 'data Entity',
    table = {
        data2 = 'data2 Entity',
    },
}
Xstates.Entity(1).state.Mystate.data = 'New data Entity'
Xstates.Entity(1).state.Mystate.table.data2 = 'New data2 Entity'

-- Player State Example :
Xstates.Player(1).state.Mystate = {
    data = 'data Player',
    table = {
        data2 = 'data2 Player',
    },
}
Xstates.Player(1).state.Mystate.data = 'New data Player'
Xstates.Player(1).state.Mystate.table.data2 = 'New data2 Player'

 (custom statebag by Xstates based on the global statebag). Note: by default, the resource statebag (without specifying a name) allows access only to data of the current resource. To read the ResourceState of another resource, you must use the explicit resource name as a parameter (e.g., Xstates.Resource('resource_name').state).
-- Resource by name State Example :
Xstates.Resource('myresource').state.Mystate = {
    data = 'data Resource',
    table = {
        data2 = 'data2 Resource',
    },
}
Xstates.Resource('myresource').state.Mystate.data = 'New data Resource'
Xstates.Resource('myresource').state.Mystate.table.data2 = 'New data2 Resource'

-- Resource State Example :
Xstates.Resource().state.Mystate = {
    data = 'data Resource',
    table = {
        data2 = 'data2 Resource',
    },
}
Xstates.Resource().state.Mystate.data = 'New data Resource'
Xstates.Resource().state.Mystate.table.data2 = 'New data2 Resource'
```
