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

#### Shared Side :

```lua
local Xstates = exports.Xstates:getSharedObject()

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
```
