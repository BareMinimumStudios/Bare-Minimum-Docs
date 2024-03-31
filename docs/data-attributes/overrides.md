---
title: Overrides
project: data-attributes
---
!!! abstract ""
    `JSON` overrides can be used to either override pre-existing attributes, either `vanilla or modded`, or if non-existant a new attribute is created. Therefore, new entity attributes can be created using just `JSON` files.

- ### Overview

Entity attributes have `four data points` that can be override using json files; see the following:

- Minimum Value `minValue`: the minimum value that this attribute’s instance is clamped to.

- Fallback Value `defaultValue`: the value that, should something go wrong, this attribute’s instance defers to (this almost never has any actual impact in-game, other than setting `initial values` if they are not otherwise provided).

- Increment Value `incrementValue`: used when calculating the diminishing returns of `DIMINISHING` attributes. Should be set to `0.0` otherwise.

- Translation Key `translationKey`: the key that gets the `display name` of the attribute.

- Stacking Behaviour `stackingBehaviour`: determines how different sources of this attribute’s value add together (either `FLAT` or `DIMINISHING`).

- #### Example 1

Minecraft’s `armor attribute` has the following:

- Default Value is `0.0`.

- Minimum Value is `0.0`.

- Maximum Value is `30.0`.

- Increment Value is `0.0`.

- Translation Key is `attribute.name.generic.armor`.

Say we want to change the maximum armor value to `100.0`. We need to know the attribute’s `registry key`, which is comprised of a `namespace` and a `pat`h. In this case, the registry key is` minecraft:generic.armor`. In the directory `data/minecraft/attributes/overrides` we create the file `generic.armor.json`. 

!!! note "Note how the registry key’s namespace matches our directory’s namespace, and the registry key’s path matches our file name."

We add the following to `generic.armor.json:`

```json
{
    "defaultValue": 0.0,
    "minValue": 0.0,
    "maxValue": 100.0,
    "incrementValue": 0.0,
    "translationKey": "attribute.name.generic.armor",
    "stackingBehaviour": "FLAT"
}
```

That’s it. By using a `Datapack` containing `data/minecraft/attributes/overrides/generic.armor.json`, the `maximum value` for armor is `100`.

- #### Example 2

Similarly to `Example 1`, the same methodology can be used to create `entirely new attributes`. 

Say we have a mod with `modid:examplemod`, and we want to add the `attribute max_mana`. We create the directory `data/examplemod/attributes/overrides/` and create the file `max_mana.json`. Inside the `JSON` file add the following:

```json
{
    "defaultValue": 0.0,
    "minValue": 0.0,
    "maxValue": 1000.0,
    "incrementValue": 0.0,
    "translationKey": "examplemod.attribute.name.max_mana",
    "stackingBehaviour": "FLAT"
}
```

We now have an `attribute registered` to the game. In our `language file` we can add the `translation key` entry like so:

```json
"examplemod.attribute.name.max_mana": "Max Mana"
```

- #### Example 3

The previous examples showed `FLAT` type `attributes` - here we see how vanilla Minecraft’s own `Knockback Resistance` attribute might be modified to be a `DIMINISHING` type:

```json
{
    "defaultValue": 0.0,
    "minValue": 0.0,
    "maxValue": 1.0,
    "incrementValue": 0.01,
    "translationKey": "attribute.name.generic.knockback_resistance",
    "stackingBehaviour": "DIMINISHING"
}
```
This file would be named `generic.knocback_resistance.jso`n and be located in `data/minecraft/attributes/overrides/`.

Although our `attribute` is `registered` to the game, it won’t be present on any `living entity yet`. The next step is to attach it to an `entity’s attribute container`.