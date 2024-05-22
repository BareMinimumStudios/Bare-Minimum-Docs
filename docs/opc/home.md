---
title: Home
project: opc
---

<!-- !!! tip ""
    **:octicons-clock-16: The Offline Player Cache documentation is currently being rewritten - it is not accurate for versions >=3.4.2**

<br> -->

# Offline Player Cache

![opc banner](../assets/opc/opc-banner.png)

!!! abstract ""
    `Offline Player Cache (OPC)` is a Minecraft API mod built using the Fabric framework to allow caching player data on the server while the player is offline. This means that mods can use this API to access offline player's data. This was primarily developed as a way to allow `global and persistent leaderboards` for Minecraft servers that included `offline players`.

- #### Specific Use Cases

`OPC` is designed to be used in conjunction with other mods. By itself it adds no content. [PlayerEx](https://modrinth.com/mod/playerex-directors-cut) uses it to cache players' levels when they disconnect from a server, so that server leaderboards that display player levels are persistent and not dependent on who is online at the time of viewing. 

Similar to a '<a href="https://readyplayerone.fandom.com/wiki/Scoreboard" target="_blank">*Ready Player One*</a>' type situation - it would be odd if Parzival disappeard from the scoreboard every time he logged off. When the player logs back in, the cached value is removed and instead the real-time value is used.

In the aforementioned case, it is only an `integer` that is `cached/used`. However, anything can be cached, so long as it can be read from/written to nbt data. Furthermore, this data is not synced across to the client, which means developers do not need to be as mindful of nbt size. 

It should be noted that the functionality provided by this mod relies on players being using `legitimate accounts` and not `cracked versions`, as their `uuid/name` must be valid for their data to be cached. 

- #### Commands

OPC registers `three commands` to access and/or remove cached data:

- `/opc get uuid|name <uuid|name> <key>` Returns the input uuid/name player's value; if they are online, returns their current value, if they are offline returns their cached value. If the value is an `instanceof` `Number` and run from a command block, the redstone output is the absolute modulus of 16.

- `/opc remove uuid|name <uuid|name> <key>` If the input uuid/name player is offline, removes that player's cached value determined by the input key; if the player is online, does nothing.

- `/opc remove uuid|name <uuid|name>` If the input uuid/name player is offline, removes all of their cached data; if the player is online, does nothing.

- #### Integrating Offline Player Cache

This mod can be accessed from its [Github](https://github.com/BareMinimumStudios/Offline-Player-Cache) or [Modrinth](https://modrinth.com/mod/opc-directors-cut) repositories; either by cloning and using a local maven repository via the `publishToMavenLocal` task, or by using the public maven repository provided by Modrinth (see: <a href="https://docs.modrinth.com/docs/tutorials/maven/" target="_blank">Modrinth Maven</a>), respectively. 

The latter is used below to show how OPC can be added to your project. In your `build.gradle` include the following:

```java
repositories {
    maven {
        name = "Modrinth"
        url = "https://api.modrinth.com/maven"
        content {
            includeGroup "maven.modrinth"
        }
    }
}
```

```java
dependencies {
    modImplementation "maven.modrinth:offline-player-cache:<version>"
    include "maven.modrinth:offline-player-cache:<version>"
}
```
OPC is designed to be included in your project jar `(jar-in-jar)`, so it is recommended to use `include`.