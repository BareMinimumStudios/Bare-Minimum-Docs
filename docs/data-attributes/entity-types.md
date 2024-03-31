---
title: Entity Types
project: data-attributes
---

!!! abstract ""
    This section covers how to `add attributes` to specific `entity types`, how to set the attribute's `default values` for that entity and how the `JSON` that does this is formatted.

- ### Overview

Although `attributes` are `registered` to the game and include a `fallback value`, attributes are `not` added to entities by default. Attributes exist independent of entities, and only when added as an _entity attribute_ instance `(like a wrapper)` is the entity able to access and manipulate its value.

Minecraft does this using two container objects: `DefaultAttributeContainer` (default attribute container) and `AttributeContaine`r (attribute container) - or similar, depending on `mappings`. 

The default attribute container is a `statically built`, `immutable object` that contains a mapping for all the attributes for a given entity type. All entities of the same entity type share the same `default attribute container`; this is also where the default values for all attributes is defined.

For entities of the same entity type to have unique values, Minecraft uses the `attribute container`. This object is yet another `wrapper`, holding the `default attribute container` and then applying/removing modifiers as needed for that entity. The attribute container is `created` when the `entity is`.

- #### Entity Types

Every entity registered to the game has an entity type, which has a registry key associated with it (such as the player, which has the registry key `minecraft:player`). This is important, as it lets as be specific about what attributes we add to what `entity types`.

- #### JSON Structure

Using `Datapacks`, the `entity_types.json` file is used to add attributes to an entity type, as well as override the default (starting) value of already added attributes. This file is synonymous with the default `attribute container`; additionally, the file acts similarly to Minecraft tags, which means that it doesn’t override other datapack `entity_types.json` files, rather it forms a collection of entries. Its contents are structured as follows:

```json
{
    "values": {
        "<entity type registry key>": {
            "<attribute registry key>": <numerical default value>,
            ...
        }
    }
}
```

- #### Example 1

Say we want to adjust the maximum health of the player to be `10` instead of the default `20`. We create the directory `data/namespace/attributes/` where `namespace` is your `modid`, or some other unique string of characters if you are making a datapack. Inside of the directory, we add the json file `entity_types.json`, and populate it with the following:

```json
{
    "values": {
        "minecraft:player": {
            "minecraft:generic.max_health": 10.0
        }
    }
}
```

!!! note "Note the presence of the full registry key for the `max health attribute`, and the full `registry key` for the entity type for the player. With that, the player will now have a `max health of 10`."

- #### Example 2

Say we have our mod, `examplemod`, and we have just created `(and registered)` the attribute `Max Mana`. We want to add it to the `witch mob` and for it to have a default value of `30`. Firstly, we know that the attribute’s registry key is `examplemod:max_mana` and the witch entity type’s registry key is `minecraft:witch`. 

We again go to the `directory data/examplemod/attributes/` and add the `JSON` file `entity_types.json`.` Inside the file we add:

```json
{
    "values": {
        "minecraft:witch": {
            "examplemod:max_mana": 30.0
        }
    }
}
```

Now the witch has the attribute `Max Mana` with a default value of `30`.

- #### Example 3

Perhaps we wish to add the `Max Mana` attribute to the `Player` as well; additionally, we want to set the default `Max Health` of the player to `10` and give Creepers `4 Armor`:

```json
{
    "values": {
        "minecraft:witch": {
            "examplemod:max_mana": 30.0
        },
        "minecraft:player": {
            "examplemod:max_mana": 30.0,
            "minecraft:generic.max_health": 10.0
        },
        "minecraft:creeper": {
            "minecraft:generic.armor": 4.0
        }
    }
}
```

- #### Additional Information

`The entity_types.json` resource is loaded after the Minecraft (and modded) default attribute containers are created. Therefore, these files will always add to and/or override the contents of the `hardcoded` default attribute containers.

The `entity_types.json` file of a given mod or datapack can be overridden by creating your `own file` in the `same directory`, and then ensuring that your datapack is `loaded last`.