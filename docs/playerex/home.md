---
title: Home
project: playerex
---

<!-- !!! tip ""
    **:octicons-clock-16: The PlayerEx documentation is currently being rewritten - it is not accurate for versions >=3.4.2**

<br> -->

# PlayerEx

![playerex banner](../../assets/playerex/banner1.png)

!!! abstract ""
    `PlayerEx` is a Minecraft mod built using the Fabric ecosystem that adds `RPG themed` attributes to the game and the player.

- #### Integrating PlayerEx

PlayerEx has a [Curseforge](https://www.curseforge.com/minecraft/mc-mods/playerex-directors-cut) and [Modrinth](https://modrinth.com/mod/playerex-directors-cut) page. To add PlayerEx to your project, insert the following into your `build.gradle`:

```groovy title="build.gradle"
repositories {
    maven {
        name = "Modrinth"
        url = "https://api.modrinth.com/maven"
        content {
            includeGroup "maven.modrinth"
        }
    }
}

dependencies {
    modImplementation "maven.modrinth:playerex-directors-cut:<version>>"
}
```
Alternatively, if you're using Cursemaven:

```groovy title="build.gradle"
repositories {
    maven {
        name = "Cursemaven"
        url = "https://cursemaven.com"
    }
}

dependencies {
    modImplementation "curse.maven:playerex-directors-cut-958325:<version-file-id>"
}
```

!!! note "PlayerEx has multiple dependencies, so your `build.gradle` must accommodate this."

- ### Dependancies

PlayerEx requires the following dependancies:

| Dependancy | Purpose |
| ------------------ | ---------------------------- |
| [Fabric API](https://github.com/FabricMC/fabric/tree/1.20.1) | Required by other dependencies and for certain events. |
| [Data Attributes: Directors Cut](https://modrinth.com/mod/data-attributes-directors-cut) | Provides the core attribute system and attribute configurability. |
| [Cardinal Components API](https://github.com/Ladysnake/Cardinal-Components-API/tree/1.20) (Base, Chunk, and Entity) | A jar-in-jar dependency that provides player attribute modifiers. |
| [Cloth Config API](https://github.com/shedaniel/cloth-config) | Provides configurability. |
| [Offline Player Cache: Directors Cut](https://modrinth.com/mod/opc-directors-cut) | A jar-in-jar dependency that provides TextPlaceholderAPI with persistent player data. |
| [Text Placeholder API](https://github.com/Patbox/TextPlaceholderAPI) | A jar-in-jar dependency that provides text placeholders, namely for custom multiplayer leaderboards. |
| [exp4j](https://www.objecthunter.net/exp4j/) | A jar-in-jar dependency that provides levelling configurability, namely an expression parser. |