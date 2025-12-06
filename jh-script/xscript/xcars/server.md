# Server

***

```lua
Xcars.getXcarByXid(Xid)
```

* Xid: <kbd>string</kbd>&#x20;
* **return** [Xcar](xcar-object.md): <kbd>self object</kbd>&#x20;



***

<pre class="language-lua"><code class="lang-lua"><strong>Xcars.getXcarByEntityId(entityId)
</strong></code></pre>

* entityId: <kbd>number</kbd>&#x20;
* **return** [Xcar](xcar-object.md): <kbd>self object</kbd>



***

```lua
Xcars.createNewXcar(properties, position, rotation, vehicleType)
```

* properties: <kbd>table</kbd>
* position: <kbd>vector3</kbd>
* rotation: <kbd>vector4</kbd>
* vehicleType: <kbd>string</kbd>
* **return** Xid: <kbd>string</kbd>, entityId: <kbd>number</kbd>



***

```lua
Xcars.createTempXcar(Xid, properties, position, rotation, vehicleType)
```

* Xid: <kbd>string</kbd> (optionnel) _temp\_..._
* properties: <kbd>table</kbd>
* position: <kbd>vector3</kbd>
* rotation: <kbd>vector4</kbd>
* vehicleType: <kbd>string</kbd>
* **return** Xid: <kbd>string</kbd>, entityId: <kbd>number</kbd>

&#x20;

***

```lua
Xcars.createXcar(Xid, dbXcar)
```

* Xid: <kbd>string</kbd>
* dbXcar: [Xcar ](xcar-object.md)<kbd>self object</kbd>&#x20;



***

<pre class="language-lua"><code class="lang-lua"><strong>Xcars.updateXcar(Xid, options)
</strong></code></pre>

* Xid: <kbd>string</kbd>
*   options: <kbd>table</kbd>

    * properties: <kbd>table</kbd> (optionnel)
    * onDb: <kbd>boolean</kbd> (optionnel)
    * freshUpdate: <kbd>boolean</kbd> (optionnel)
    * vehicleType: <kbd>string</kbd> (optionnel)
    * enable: <kbd>boolean</kbd> (optionnel)



***

```lua
Xcars.deleteXcar(Xid, options)
```

* Xid: <kbd>string</kbd>
*   options: <kbd>table</kbd>

    * properties: <kbd>table</kbd> (optionnel)
    * restarting: <kbd>boolean</kbd> (optionnel)
    * onDb: <kbd>boolean</kbd> (optionnel)
    * freshUpdate: <kbd>boolean</kbd> (optionnel)



***

```lua
Xcars.reapplyProperties(Xid, options)
```

* Xid: <kbd>string</kbd>
*   options: <kbd>table</kbd>

    * properties: <kbd>table</kbd>
    * autoUpdate: <kbd>boolean</kbd> (optionnel)



***

```lua
Xcars.tryGetNewXcarData()
```

* **return**: <kbd>table</kbd>&#x20;
  * is\_full\_fresh\_data: <kbd>boolean</kbd>
  * properties: <kbd>table</kbd>
  * position: <kbd>vector3</kbd>
  * rotation: <kbd>vector4</kbd>
  * vehicleType: <kbd>string</kbd>
