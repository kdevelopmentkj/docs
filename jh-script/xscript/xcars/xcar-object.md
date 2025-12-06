# Xcar object

***

<pre><code><strong>Xcar
</strong></code></pre>

* entityId: <kbd>number</kbd>
* Xid: <kbd>string</kbd>
* properties: <kbd>table</kbd>
* position: <kbd>vector3</kbd>
* rotation: <kbd>vector4</kbd>
* vehicleType: <kbd>string</kbd>
* enable: <kbd>boolean</kbd>&#x20;



***

<pre class="language-lua"><code class="lang-lua"><strong>Xcar.updateXcar(options)
</strong></code></pre>

*   options: <kbd>table</kbd>

    * properties: <kbd>table</kbd> (optionnel)
    * onDb: <kbd>boolean</kbd> (optionnel)
    * freshUpdate: <kbd>boolean</kbd> (optionnel)
    * vehicleType: <kbd>string</kbd> (optionnel)
    * enable: <kbd>boolean</kbd> (optionnel)



***

```lua
Xcar.deleteXcar(options)
```

*   options: <kbd>table</kbd>

    * properties: <kbd>table</kbd> (optionnel)
    * restarting: <kbd>boolean</kbd> (optionnel)
    * onDb: <kbd>boolean</kbd> (optionnel)
    * freshUpdate: <kbd>boolean</kbd> (optionnel)



***

```lua
Xcar.reapplyProperties(options)
```

*   options: <kbd>table</kbd>

    * properties: <kbd>table</kbd>
    * autoUpdate: <kbd>boolean</kbd> (optionnel)



***

```lua
Xcar.tryGetNewXcarData()
```

* **return**: <kbd>table</kbd>&#x20;
  * is\_full\_fresh\_data: <kbd>boolean</kbd>
  * properties: <kbd>table</kbd>
  * position: <kbd>vector3</kbd>
  * rotation: <kbd>vector4</kbd>
  * vehicleType: <kbd>string</kbd>
