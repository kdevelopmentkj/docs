# Configuration

The configuration file is located at `shared/config.lua` and can be modified directly.

**Available options :**

* **`SYNC_DELAY`** : <kbd>number</kbd>
  * The delay in milliseconds between each sync server -> client
  * Default: `100`

* **`PrintFailedSecurityKey`** : <kbd>boolean</kbd>
  * If `true`, the server will print a message if the security key is failed
  * Default: `true`

* **`AuthorizedClientSideEditing_GlobalState`** : <kbd>boolean</kbd>
  * If `true`, the client can edit the global statebag
  * Default: `false`

* **`AuthorizedClientSideEditing_ResourceState`** : <kbd>boolean</kbd>
  * If `true`, the client can edit the resource statebag
  * Default: `false`
