# Xstates

{% embed url="https://kdev-jh.tebex.io/package/xstates" %}

**Xstates** is an advanced state bag system for FiveM that extends the native `GlobalState` functionality with powerful features like direct table modification, automatic debouncing, deep table support, and circular reference handling.

> **"Why rewrite the entire table when you can just modify what you need? Xstates: The intelligent way to manage shared state."**

<table><thead><tr><th width="200">Operations</th><th width="180">Fivem State bags</th><th width="180">Xstates State bags</th><th>Advantage</th></tr></thead><tbody><tr><td>10 rapid changes</td><td>10 network saves</td><td>1 batched save</td><td>10x fewer calls</td></tr><tr><td>Modify nested value</td><td>3 lines, full rewrite</td><td>1 line, direct</td><td>3x less code</td></tr><tr><td>Deep table (20 levels)</td><td>❌ Fails</td><td>✅ Works</td><td>Unlimited depth</td></tr><tr><td>Circular references</td><td>❌ May fail</td><td>✅ Handled</td><td>Safe serialization</td></tr><tr><td>Metatable preservation</td><td>❌ Lost</td><td>✅ Preserved</td><td>Full compatibility</td></tr><tr><td>Smart cache</td><td>❌ None</td><td>✅ Yes</td><td>No extra exchange</td></tr></tbody></table>

<details>

<summary>⚠️ Important Notes</summary>

**Debounce Timing :**

* Changes are saved automatically after 1 second of inactivity
* Rapid successive changes are batched together
* Use <kbd>.set()</kbd> method if you need immediate save _(bypasses debounce)_

**Compatibility :**

* 100% API compatible with native FiveM state bags
* Drop-in replacement - just replace <kbd>GlobalState</kbd> with <kbd>Xstates.GlobalState</kbd>
* Works alongside native state bags (you can use both)

</details>

***

**Dependence :**

* onesync
