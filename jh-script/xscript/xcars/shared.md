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

# Shared

***

```lua
Xcars.toJSON()
```

* **return** <kbd>table</kbd> _(Protected table and not printable if not use toJSON)_



***

```lua
Xcars.onEvent(eventName, handler)
```

(only shared if XcarsConfig.onlyServerSideEvents = false else only server)

* eventName: <kbd>string</kbd> _(createVehicle, updateVehicle, deleteVehicle)_
*   handler: <kbd>function</kbd>&#x20;



    ```lua
    Xcars.onEvent('createVehicle', function(newState)
    end)

    Xcars.onEvent('deleteVehicle', function(lastState, newState)
    end)

    Xcars.onEvent('updateVehicle', function(lastState, newState)
    end)
    ```

