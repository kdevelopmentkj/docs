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

# Xgroups

{% embed url="https://kdev-jh.tebex.io/package/xgroups" %}

**Xgroups** is a complete and high-performance group and permission management system for your FiveM servers. Designed to be flexible, powerful, and easy to integrate, it allows you to efficiently manage

#### 🏗️ Modular Architecture

* **Hierarchical group system**: Create groups with multiple ranks
* **Granular permissions**: Precisely control access for each rank

#### 🚀 Optimized Performance

* **Smart caching system**: Drastically reduces database queries
* **Automatic cache**: Data is cached for 30 minutes after a player disconnects
* **Automatic cleanup**: Cache cleans itself to prevent memory leaks
* **Optimized queries**: Architecture designed with performance in mind

#### 🔐 Advanced Permission System

* **Wildcards**: Use `*` to grant all permissions
* **Prefix wildcards**: Use `prefix.*` to grant access to all permissions under a prefix
* **Group wildcards**: Use `group:*` to grant all permissions of a group
* **Specific permissions**: Precise control with `group:permission.key` &#x20;



_All operations in the system are fully **typesafe** and protected using **pcall**, ensuring that unexpected issues never break the execution flow. If any error occurs during the creation or modification of groups, ranks, permissions, or any other element, the system automatically **rolls back** to the last valid save. This guarantees data integrity and prevents corrupted configurations, even in the event of unforeseen failures._



<details>

<summary>🎮 Features</summary>

**Group Management**

* ✅ Create/Delete groups
* ✅ Add/Remove ranks within a group
* ✅ Manage permissions per group
* ✅ Assign/Remove permissions for ranks

**Player Management**

* ✅ Assign groups and ranks to players
* ✅ Remove groups from players
* ✅ Admin bypass system
* ✅ Retrieve a player's groups
* ✅ Retrieve a player's perms

**Permission Checking**

* ✅ Server-side checking
* ✅ Client-side checking
* ✅ Support for hierarchical permissions
* ✅ Bypass verification

**Optimised**

* ✅ client and server 0.00ms&#x20;
* ✅ hasPermission returns \~0.30ms (tested with 1000 groups × 1000 ranks × 1000 permissions)

</details>

***

**Dependence :**

* onesync
* oxmysql

#### 🔗 Compatibility

Xgroups is compatible with the following frameworks :

* ESX
* QBCore
* OXCore
* Standalone
