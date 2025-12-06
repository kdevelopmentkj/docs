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

<p align="center"><strong>Available only if XcarsConfig.onlyServerSideControl = false</strong> </p>

***

```lua
Xcars.updateVehicle(entityId, options)
```

* entityId: <kbd>number</kbd>
* options: <kbd>table</kbd>
  * properties: <kbd>table</kbd> (optionnel)
  * onDb: <kbd>boolean</kbd> (optionnel)
  * freshUpdate: <kbd>boolean</kbd> (optionnel)
  * vehicleType: <kbd>string</kbd> (optionnel)
  * enable: <kbd>boolean</kbd> (optionnel)



***

```lua
Xcars.deleteVehicle(entityId, options)
```

* entityId: <kbd>number</kbd>
* options: <kbd>table</kbd>
  * properties: <kbd>table</kbd> (optionnel)
  * restarting: <kbd>boolean</kbd> (optionnel)
  * onDb: <kbd>boolean</kbd> (optionnel)
  * freshUpdate: <kbd>boolean</kbd> (optionnel)
