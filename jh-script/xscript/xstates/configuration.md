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

# Configuration

The configuration file is located at `shared/config.lua` and can be modified directly.

**Available options :**

* **`SYNC_DELAY`** : `number`
  * The delay in milliseconds between each sync server -> client
  * Default: `100`
* **`PrintFailedSecurityKey`** : `boolean`
  * If `true`, the server will print a message if the security key is failed
  * Default: `true`
* **`AuthorizedClientSideEditing_GlobalState`** : `boolean`
  * If `true`, the client can edit the global statebag
  * Default: `false`
* **`AuthorizedClientSideEditing_ResourceState`** : `boolean`
  * If `true`, the client can edit the resource statebag
  * Default: `false`
