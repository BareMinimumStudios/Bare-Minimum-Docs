---
title: Overrides
project: data-attributes
---
!!! abstract ""
    `JSON` overrides can be used to either override pre-existing attributes, either `vanilla or modded`, or if non-existant a new attribute is created. Therefore, new entity attributes can be created using just `JSON` files.

### Overview

Entity attributes have `four data points` that can be override using json files; see the following:

- **`enabled`**: Whether to enable an override or not.

- **`min`**: The minimum value that this attribute’s instance is clamped to.

- **`max`**: The maximum value that this attribute’s instance is clamped to.

- **`smoothness`**: Used when calculating the diminishing returns of **`Diminished`** attributes.
  - When an override is set to **`Flat`**, this does not do anything.

- **`min_fallback`**: The deferred and prior minimum value that this attribute’s instance is clamped to.

- **`max_fallback`**: The deferred and prior maximum value that this attribute’s instance is clamped to.

- **`formula`**: Determines how different sources of the attribute value add together (**`Flat`** or **`Diminished`**).
  - For **diminished** values, it is advised to use ranges from at least -1 to at most 1.

- **`format`**: Determines how the API formats the value when using the **`DataAttributesAPI#getFormattedValue`** function (**`Flat`** or **`Percentage`**).

#### Example 1

Minecraft’s **Armor** attribute (aka `minecraft:generic.armor`) has the following:

- Its base value is `0.0`.

- It has a minimum of `0.0`, and a maximum of `30.0`.

Say we want to change the maximum armor value to `100.0`. We need to know the attribute’s `registry key`, which is comprised of a `namespace` and a `path`. In this case, the registry key is `minecraft:generic.armor`.

For data-packs, in the directory `data/<namespace>/data_attributes/overrides` we create an overrides file with any overrided entries we want, and you can insert an armor entry.

For configs, you will have to place an armor entry in the `overrides.json5` or use the UI to add an entry targeting the armor attribute.

```json
"minecraft:generic.armor": {
    "enabled": true,
    "min": 0.0,
    "max": 100.0,
    "smoothness": 1.0,
    "min_fallback": 0.0,
    "max_fallback": 30.0,
    "formula": "Flat",
    "format": "Whole"
}
```

That’s it. Upon applying the data-pack or config, the `maximum value` for armor should be `100`.

#### Example 2

Here we see how vanilla Minecraft’s own **`Knockback Resistance`** attribute might be modified to be a **`Diminished`** type, with the **`Percentage`** format:

```json
"minecraft:generic.knockback_resistance": {
    "enabled": true,
    "min": 0.0,
    "max": 1.0,
    "smoothness": 0.01,
    "formula": "Diminished",
    "format": "Percentage"
}
```

!!! note "Although our attribute is registered to the game, it won’t be present on any `LivingEntity` just yet. The next step is to attach it to an `EntityAttribute`'s container."
