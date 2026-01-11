# Configuration

The configuration file is located at `config.lua` and can be modified directly.

**Available options :**

* **`DistanceToLoad`** : `number`
  * The distance in meters at which containers will be loaded
  * Default: `150`

* **`CooldownTime`** : `number`
  * The cooldown time in milliseconds before a container can be opened again
  * Default: `15 * 60 * 1000` (15 minutes)

* **`Marker`** : `table`
  * Type: `number` - Marker type (default: `23`)
  * Color: `table` - Marker color RGBA
    * r: `number` - Red component (0-255)
    * g: `number` - Green component (0-255)
    * b: `number` - Blue component (0-255)
    * a: `number` - Alpha component (0-255)
  * Scale: `number` - Marker scale (default: `0.7`)

* **`Containers`** : `table`
  * Array of zones, each zone containing an array of containers
  * Each container has:
    * coords: `vec4` - Container position (x, y, z, heading)
    * model: `string` - Container model type
      * Available models: `"small"`, `"medium"`, `"large"`
    * color: `string` - Container color
      * Available colors: `"random"`, `"red"`, `"green"`, `"orange"`, `"blue"`
