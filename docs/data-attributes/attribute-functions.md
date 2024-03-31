---
title: Attribute Functions
project: data-attributes
---

!!! abstract ""
    Data Attributes introduces two entirely new ideas to Minecraft's entity attributes: `functions` and `properties`. This section covers attribute functions by presenting a series of examples.

- #### Overview

`Attribute Functions` allow you to relate attributes to each other in a way that is common in other games: `Dota `2 has the primary attributes `Strength, Agility and Intelligence` - where `Strength` increases the `health pool`, `Agility` increases `armor` and `Intelligence` increases `Mana`. `PoE` is much the same, where `Strength increases maximum life`, `Dexterity` grants `Evasion` and `Intelligence` also `increases Mana`.

The implementation of `Attribute Functions` is out of the scope of this wiki, but basically each attribute now has a collection of children and parent attributes. When defining an `Attribute Function`, you define an entity attribute `(the parent)` and assign to it a list of other attributes `(children)`. The resulting behaviour in-game is that whenever the value of a `parent attribute changes`, the value of all its `children changes` as well.

!!! note "This is one-directional, a change in the value of a child does not impact the value of its parents."

- #### The Need for Attribute Functions

The premise for this questions resides in a commonly found aspect of `RPGs`. Often, their attribute systems are structured with a few primary attributes `(think Constitution, Strength, Dexterity for example)` and then secondary attributes `(think Max Health, Damage, Evasion for example)`. Increasing the value of a `primary attribute` increases the value of `secondary attributes` as well.

- #### The Problem

Lets say we have a Minecraft mod that provides this `RPG-type attribute system`, and there is a nice screen with `buttons` that let you skill/increase those primary attributes. In code, we may have a method similar to `Button#addAttribute`; we would pass the attribute to increase and the amount to increase it by as arguments:

```java
void onButtonClick(Button button) {
    button.addAttribute(Attributes.CONSTITUTION, 1.0);
}
```

This is actually fine so far; there is nothing wrong with doing it this way. However, things get cumbersome when we implement `follow-on attributes`. Say for every one point in `Constitution`, we increase our `Max Health` by `one`, `Knockback Resistance` by `0.01` and `Damage` by `0.5`:

```java
void onButtonClick(Button button) {
    button.addAttribute(Attributes.CONSTITUTION, 1.0);
    button.addAttribute(Attributes.MAX_HEALTH, 1.0);
    button.addAttribute(Attributes.KNOCKBACK_RESISTANCE, 0.01);
    button.addAttribute(Attributes.DAMAGE, 0.5);
}
```

This is not great. Furthermore, what happens when someone uses the `/attribute` command to increase their `Constitution?` Or equips an item that increases their `Constitution?` Or what happens when a player wants to customise what attributes are changed when Constitution is changed? What about `configurability?` We have to create unwieldy `repetitons` everywhere any kind of attribute modifying code occurs. It is not feasible to do this.

- #### The Solution

The solution is described briefly in the `Overview`: we denominate our attributes as either the `parent or child`; to the parent, we `attach` a collection of references to it's `children`; and to the child, we `attach` a collection of references to it's `parents`.

!!! note "Denominations of child and parent are not `mutually exclusive`. A parent attribute can be a child to another attribute, just as a child attribute can be a parent to another attribute."

- #### Example 1

Say the player has the attribute `Constitution`; and for every point added in this attribute, a point is also added to `Max Health`. This would be done using an `attribute function`. Functions allow for attributes to influence each other.

We have our `examplemod`, and we’ve added the attribute `Constitution`, but now we want to make it so that for every point added to `Constitution`, a point is added to `Max Health` and half a point is added to `Armor`.

We go to the directory `data/examplemod/attributes/` where we create the `JSON` file functions.json. Inside this file, we add the following:

```json
{
    "values": {
        "examplemod:constitution": {
            "minecraft:generic.max_health": {
                "behaviour": "ADDITION",
                "value": 1.0
            },
            "minecraft:generic.armor": {
                "behaviour": "ADDITION",
                "value": 0.5
            }
        }
    }
}
```

They are now differentiated by Function Behaviour as either `ADDITION` or `MULTIPLY` functions. This is due to the need to be able to have attributes that contribute `multiplicatively` instead of `flat addition`.

Instead of a `number value` assigned to the attribute, an `object value` is assigned that contains the `behaviour` and `value` fields.

- #### Example 2

```json
{
    "values": {
        "examplemod:dexterity": {
            "minecraft:generic.attack_speed": {
                "behaviour": "MULTIPLY",
                "value": 0.02
            },
            "examplemod:dash_speed": {
                "behaviour": "ADDITION",
                "value": 0.1
            }
        }
    }
}
```
Every point in `examplemod:dexterity` now increases `Attack Speed` by `2%` - or more specifically, `multiplies` the total value of `Attack Speed` by `1.02` = `1.0 + 0.02`.

- ### Recursion

The keen eye among you may have notices that there is an opportunity for `recursion/infinite attributes`. Lets show how this might be done:

```json
{
    "values": {
        "examplemod:constitution": {
            "minecraft:generic.max_health": 1.0
        },
        "minecraft:generic.max_health": {
            "examplemod:constitution": 1.0
        }
    }
}
```
Looking at our `functions.json` file, we can see that when we add a point to `Constitution`, a point is added to `Max Health`; but when a point is added to `Max Health`, a point is added to `Constitution` - it would seem we have a vicious cycle on our hands. 

One could even see how a huge chain of attributes could accidentally become `recursive`. `Constitution` adds to `Max Health` adds to `Strength` adds to `Armor` adds to `Knockback Resistance` adds to `Constitution… ` Not to mention that since each attribute can have many functions attached, the tree of possible combinations resulting in `recursion` can be `extensive`.

Therefore, `Data Attributes` implements protections against this. When the `functions.json` files are `read and assembled`, the functions tree is `scanned for loops`. Any loops found are broken by removing the first function found that results in a loop. 

The specific function that would be `removed` for a specific set of circumstances is unreliable, just that it is impossible for loops to form. This is done so that checks do not have to be made in game, which could cause lag.

That being said, developers and pack creators alike should still be careful not to `create loops`.