---
title: API
project: playerex
---

- #### Structure and Overview

PlayerEx includes an API package, located at `com.github.clevernucleus.playerex.api` and contains the following:

```
📂api
 ┣📂client
 ┃ ┣📄ClientUtil.java class
 ┃ ┣📄PageLayer.java class
 ┃ ┣📄PageRegistry.java class
 ┃ ┗📄RenderComponent.java class
 ┃
 ┣📂damage
 ┃ ┣📄DamageFunction.java interface
 ┃ ┗📄DamagePredicate.java interface
 ┃
 ┣📂event
 ┃ ┣📄LivingEntityEvents.java class
 ┃ ┗📄PlayerEntityEvents.java class
 ┃
 ┣📄EntityAttributeSupplier.java class
 ┣📄ExAPI.java class
 ┣📄ExConfig.java interface
 ┣📄PacketType.java enum
 ┗📄PlayerData.java interface
```

 The `PlayerEx API` can be split into four distinct sub-systems: the `client-side page/tab` registry, which exists to allow developers to add more inventory tabs and content to existing inventory pages; the damage-registry, which exists to allow developers to register damage handlers to better manipulate damage events; the `LivingEntityEvents` and `PlayerEntityEvents` classes, which provide developers useful hooks into attributes and specific methods; and finally, the attribute registry - which acts to provide developers with attribute suppliers and access to all the attributes `PlayerEx` adds.

 - #### API Content

 The following subsections detail every method/field in each java class located in the `api` package.

- ### client.ClientUtil.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public` `static` `final` `DecimalFormat` | **FORMATTING_2** Format for two decimal places. |
| `public` `static` `final` `DecimalFormat` | **FORMATTING_3** Format for three decimal places. |
| `public` `static` `double` | **displayValue**(**Supplier&lt;EntityAttribute&gt;** attribute, **double** value) Takes the input attribute and looks for the `PERCENTAGE_PROPERTY` property; if it is present, multiplies the input value by the property's multiplier. If not, then looks for the `MULTIPLIER_PROPERTY` property; if present, multiplies the input value by the property's multiplier. Else returns the input value. |
| `public` `static` `String` | **formatValue**(**Supplier&lt;EntityAttribute&gt;** attribute, **double** value) If the input value is positive, adds the `+` prefix. If the input attribute has the `PERCENTAGE_PROPERTY` property, appends the `%` suffix. |
| `public` `static` `void` | **appendChildrenToTooltip**(**List&lt;Text&gt;** tooltip, **Supplier&lt;EntityAttribute&gt;** attribute) Looks for the input attribute's functions, and if there are any, formats them and adds them to the input list as tooltips. |
| `public` `static` `void` | **modifyAttributes**(**PacketType** type, **Consumer&lt;BiConsumer&lt;EntityAttributeSupplier, Double&gt;&gt;** ... consumers) Client-side. Sends a packet to the server to modify the attribute modifiers provided by PlayerEx by the input amount, and performs the checks and functions dictated by the PacketType (this logic is run on the server when it receives the packet). |

- ### client.PageLayer.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `protected` `final` `HandledScreen<?>` | **parent** Reference to the parent screen. |
| `public` `void` | **render**(**MatrixStack** matrices, **int** mouseX, **int** mouseY, **float** delta) This is where text and tooltips can be rendered, in that order. |
| `public` `void` | **drawBackground**(**MatrixStack** matrices, **float** delta, **int** mouseX, **int** mouseY) This is where textures and widgets can be rendered, in that order. |

- ### client.PageRegistry.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public` `static` `void` | **registerPage**(**Identifier** pageId, **Identifier** icon, **Identifier** texture, **Text** title) Registers a page and tab to the PlayerEx screen. |
| `public` `static` `void` | **registerPage**(**Identifier** pageId, **Identifier** icon, **Text** title) Registers a page and tab to the PlayerEx screen, but with the default background. |
| `public` `static` `void` | **registerLayer**(**Identifier** pageId, **PageLayer.Builder** builder) Registers a page layer to a page of the same input pageId. |

- ### client.RenderComponent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public` `static` `RenderComponent` | **of**(**Supplier&lt;EntityAttribute&gt;** attribute, **Function&lt;LivingEntity, Text&gt;** function, **Function&lt;LivingEntity, List&lt;Text&gt;&gt;** tooltip, **int** dx, **int** dy) Creates a new `RenderComponent` object. |
| `public` `static` `RenderComponent` | **of**(**Function&lt;LivingEntity, Text&gt;** function, **Function&lt;LivingEntity, List&lt;Text&gt;&gt;** tooltip, **int** dx, **int** dy) Creates a new `RenderComponent` object. |
| `private` `boolean` | **isMouseOver**(**float** x, **float** y, **float** width, **float** height, **int** mouseX, **int** mouseY) Checks to see if the mouse is over the input area. |
| `public` `void` | **renderText**(**LivingEntity** livingEntity, **MatrixStack** matrices, **TextRenderer** textRenderer, **int** x, **int** y, **float** scaleX, **float** scaleY) Renders the text from the input function. |
| `public` `void` | **renderTooltip**(**LivingEntity** livingEntity, **RenderTooltip** consumer, **MatrixStack** matrices, **TextRenderer** textRenderer, **int** x, **int** y, **int** mouseX, **int** mouseY, **float** scaleX, **float** scaleY) Renders tooltip for the given text. |

- ### damage.DamageFunction.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `float` | **apply**(**LivingEntity** livingEntity, **DamageSource** source, **float** damage) Using the input conditions, modifies the incoming damage (either reducing or increasing it) and returns the result. |

- ### damage.DamagePredicate.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `boolean` | **test**(**LivingEntity** livingEntity, **DamageSource** source, **float** damage) Determines, using the input conditions, whether to apply the `DamageFunction` to the incoming damage of the `LivingEntity`. |

- ### event.LivingEntityEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `public` `static` `final` `Event<Healed>` | **ON_HEAL** Fired before `LivingEntity#heal`; allows the amount healed to be modified before healing happens. Setting the output to 0 is an unreliable way to negate incoming damage depending on other mods installed. Instead, use `SHOULD_HEAL`. |
|  `public` `static` `final` `Event<Heal>` | **SHOULD_HEAL** Fired at the start of `LivingEntity#heal`, but before healing is applied. Can return false to cancel all healing, or true to allow it. |
|  `public` `static` `final` `Event<Tick>` | **EVERY_SECOND** Fired once at the end of `LivingEntity#tick`, every 20 ticks (1 second). |
|  `public` `static` `final` `Event<Damaged>` | **ON_DAMAGE** Fired before `LivingEntity#damage`; allows the amount of damage to be modified before it is used in any way. Can be used to perform logic prior to the damage method, and can return the original damage to avoid modifying the value. The original value is the incoming damage, followed by the result of this event by any previous registries. Setting the output to 0 is an unreliable way to negate incoming damage depending on other mods installed. Instead, use `SHOULD_DAMAGE`. |
|  `public` `static` `final` `Event<Damage>` | **SHOULD_DAMAGE** Fired after: `LivingEntity#isInvulnerableTo``World#isClient``LivingEntity#isDead``DamageSource#isFire` and `LivingEntity#hasStatusEffect``LivingEntity#isSleeping`but before all other logic is performed. Can be used to cancel the method and prevent damage from being taken by returning false. Returning true allows the logic to continue. |
|  `public` `interface` | **Healed** {...} Nested functional interface for `ON_HEAL` . |
|  `public` `interface` | **Heal** {...} Nested functional interface for `SHOULD_HEAL` . |
|  `public` `interface` | **Tick** {...} Nested functional interface for `EVERY_SECOND` . |
|  `public` `interface` | **Damaged** {...} Nested functional interface for `ON_DAMAGE` . |
|  `public` `interface` | **Damage** {...} Nested functional interface for `SHOULD_DAMAGE` . |

- ### event.PlayerEntityEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `public` `static` `final` `Event<AttackCritDamage>` | **ON_CRIT** Fired after the player lands a critical hit. The result is the damage. |
|  `public` `static` `final` `Event<AttackCritBoolean>` | **SHOULD_CRIT** Fired when determining if the player's attack is critical. Return true if it is critical, return false if it is not. |
|  `public` `interface` | **AttackCritDamage** {...} Nested functional interface for `ON_DAMAGE` . |
|  `public` `interface` | **AttackCritBoolean** {...} Nested functional interface for `SHOULD_DAMAGE` . |

- ### EntityAttributeSupplier.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `private` `final` `Identifier` | **identifier** The attribute's identifier (data does not change, objects do). |
|  `public` `static` `EntityAttributeSupplier` | **of**(**Identifier** registryKey) Creates a new `EntityAttributeSupplier` object. |
|  `public` `Identifier` | **getId**() Returns the supplier's attribute registry key. |
|  `public` `EntityAttribute` | **get**() Returns the mapped `EntityAttribute` object in the games attribute registry if it exists for this supplier's registry key; otherwise, returns null. |

- ### ExAPI.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `public` `static` `final` `String` | **MODID** The PlayerEx mod id. |
|  `public` `static` `final` `String` | **INTEGER_PROPERTY** Round value to integer property that is attached to some attributes. |
|  `public` `static` `final` `String` | **PERCENTAGE_PROPERTY** Multiply value by property and append % symbol. |
|  `public` `static` `final` `String` | **MULTIPLY_PROPERTY** Multiply value by property. |
|  `public` `static` `final` `UUID` | **PLAYEREX_MODIFIER_ID** The UUID PlayerEx attribute modifiers use. |
|  `public` `static` `final` `CacheableValue<Integer>` | **LEVEL_VALUE** The `CacheableValue` key for PlayerEx's player data.. |
|  `public` `static` `final` `ComponentKey<PlayerData>` | **PLAYER_DATA** The Caridnal Components Key for PlayerEx player data. |
|  `public` `static` `final` `EntityAttributeSupplier` | **LEVEL** The level attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **CONSTITUTION** The constitution attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **STRENGTH** The strength attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **DEXTERITY** The dexterity attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **INTELLIGENCE** The intelligence attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **LUCKINESS** The luckiness attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **EVASION** The evasion attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **LIFESTEAL** The lifesteal attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **HEALTH_REGENERATION** The health regeneration attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **HEAL_AMPLIFICATION** The heal amplification attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **MELEE_CRIT_DAMAGE** The melee crit damage attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **MELEE_CRIT_CHANCE** The melee crit chance attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **RANGED_CRIT_DAMAGE** The ranged crit damage attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **RANGED_CRIT_CHANCE** The ranged crit chance attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **RANGED_DAMAGE** The ranged damage attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **FIRE_RESISTANCE** The fire resistance attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **FREEZE_RESISTANCE** The freeze resistance attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **LIGHTNING_RESISTANCE** The lightning resistance attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **POISON_RESISTANCE** The poison resistance attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **WITHER_RESISTANCE** The wither resistance attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **BREAKING_SPEED** The breaking speed attribute. |
|  `public` `static` `final` `EntityAttributeSupplier` | **REACH_DISTANCE** The reach distance attribute. This is not implemented by PlayerEx, but by JamieWhiteShirt's reach-entity-attributes. |
|  `public` `static` `final` `EntityAttributeSupplier` | **ATTACK_RANGE** The attack range attribute. This is not implemented by PlayerEx, but by JamieWhiteShirt's reach-entity-attributes. |
|  `public` `static` `ExConfig` | **getConfig**() Access to PlayerEx's config. |
|  `public` `static` `void` | **registerDamageModification**(**DamagePredicate** predicate, **DamageFunction** function) Registers a damage modification condition that is applied to living entities under specific circumstances. |
|  `public` `static` `void` | **registerRefundCondition**(**BiFunction&lt;PlayerData, PlayerEntity&gt;** refundCondition) Registers a refund condition. Refund conditions tell the game what can be refunded and what the maximum number of refund points are for a given circumstance. |

- ### ExConfig.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `boolean` | **resetOnDeath**() |
|  `boolean` | **isAttributesGuiDisabled**() |
|  `int` | **skillPointsPerLevelUp**() |
|  `int` | **requiredXp**(**PlayerEntity** player) |
|  `float` | **levelUpVolume**() |
|  `float` | **skillUpVolume**() |
|  `float` | **textScaleX**() |
|  `float` | **textScaleY**() |

- ### PacketType.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
|  `public` `static` `final` `PacketType` | **DEFAULT**Does nothing extra - just allows the server to modify PlayerEx's attribute modifiers. |
|  `public` `static` `final` `PacketType` | **LEVEL**Only allows modification if the player's experience levels are greater than or equal to the required xp to level up. Also removes those experience levels from the player and adds n skill points; where n is defined by config option skill points per level up. |
|  `public` `static` `final` `PacketType` | **SKILL**Only allows modification if the player's skill points are greater than or equal to 1. Also subtracts one skill point from the player. |
|  `public` `static` `final` `PacketType` | **REFUND**Only allows modification if the player's refund points are greater than or equal to 1. Also subtracts one refund point and adds one skill point. |
|  `public` `static` `PacketType` | **fromId**(**byte** id)Gets the correct `PacketType` from the input (or else `DEFAULT`). |
|  `public` `byte` | **id**()Returns the `PacketType`'s id. |
|  `public` `boolean` | **test**(**MinecraftServer** server, **ServerPlayerEntity** player, **PlayerData** data)Runs the test function. |
