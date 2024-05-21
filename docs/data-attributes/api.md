---
title: API
project: data-attributes
---

- #### Structure and Overview

Data Attributes includes an API package, located at `com.github.clevernucleus.dataattributes.api` and contains the following:

```
📂api
 ┣📂attribute
 ┃ ┣📄AdditionFunction.java interface
 ┃ ┣📄AttributeBehaviour.java enum
 ┃ ┣📄AdditionFunction.java interface
 ┃ ┣📄IAttribute.java interface
 ┃ ┣📄IAttributeFunction.java interface
 ┃ ┗📄IEntityAttributeModifier.java interface
 ┃
 ┣📂event
 ┃ ┣📂client
 ┃ ┃ ┗📄ClientSyncedEvent.java class
 ┃ ┃
 ┃ ┣📄EntityAttributeEvents.java class
 ┃ ┣📄MathClampEvent.java class
 ┃ ┗📄ServerSyncedEvent.java class
 ┃
 ┗📄API.java class
```

- ### API Content

The following `subsections` detail every `method/field` in each java class located in the `api` package.

- ##### attribute.IEntityAttribute.java

| Modifiers and Type | Method/Field and Description |
| -----------        | -----------------------------|
| `double`           | `minValue()` - Returns the attribute’s minimum value. |
| `double`           | `maxValue()` - Returns the attribute’s maximum value. |
| `StackingBehaviour`| `stackingBehaviour()` - Returns the attribute’s stacking behaviour enum type. |
| `Map<IEntityAttribute, Double>` | `children()` - Returns an immutable map of the function-children attached to this attribute. |
| `Collection<String>` | `properties()` - Returns an immutable collection of the properties’ keys attached to this attribute. |
| `boolean` | `hasProperty(String property)` - Returns `true` if this attribute has the input property key, `false` if not or if the input is null. |
| `String` | `getProperty(String property)` - Returns the attribute’s property value mapped to the input key. If it does not exist or is null, returns `""`. |

- ##### attribute.IEntityAttributeInstance.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `void` | `updateModifier(UUID uuid, double value)` - Changes the value of the input modifier (if it exists) and updates the instance and all children. |

- ##### attribute.IItemEntityAttributeModifiers.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `default Multimap<EntityAttribute, EntityAttributeModifier>` | `getAttributeModifiers(ItemStack stack, EquipmentSlot slot)` - This method provides a mutable attribute modifier multimap so that items can have dynamically changing modifiers based on nbt. By default returns an empty `HashMultimap`. |

- ##### attribute.StackingBehaviour.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `publicstatic final StackingBehaviour` | `FLAT` - Flat/normal stacking behaviour. |
| `publicstatic final StackingBehaviour` | `DIMINISHING` - diminishing/normal stacking behaviour. |
| `public byte` | `id()` - Returns the byte id of the behaviour enum. |
| `public double` | `max(double current, double input)` - Clamps the input and returns the greater of the current or absolute clamped input. |
| `public double` | `stack(double current, double input)` - Clamps the input and returns the current + the absolute clamped input. |
| `public double` | `result(double k, double k2, double v, double v2, double increment)` - Returns the inputs through the correct function for the specific behaviour type. |
| `public static StackingBehaviour` | `of(byte id)` - Takes a byte id and retusn the corresponding behaviour type. |

- ##### attribute.StackingFunction.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `double` | `result(double k, double k2, double v, double v2, double increment)` - Returns the inputs through the correct function for the specific behaviour type. |

- ##### event.EntityAttributeModifiedEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public static final Event<Reloaded>` | `EVENT` - Fires when the value of an attribute instance has been modified, either by adding/removing a modifier or changing the value of a modifier. |
| `public static final Event<Clamp>` | `CLAMPED` - Fires after the attribute instance value is calculated, but before it is output. This offers one last chance to alter the value in some way (for example round a decimal to an integer). |
| `public interface` | `Modified {…}` - Nested functional interface for `MODIFIED`. |
| `public interface` | `Reloaded {…}` - Nested functional interface for `EVENT`. |

- ##### event.AttributesReloadedEvent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public static final Event<Reloaded>` | `EVENT` - Event that allows for logic reliant on datapack attributes to be ordered after they are loaded on both the server and client. Fired on Server upon joining world and on datapack reload through `/reload`. Fired on Client when selecting datapacks and after server has synced with client. |
| `public interface` | `Reloaded {…}` - Nested functional interface for `EVENT`. |

- ##### item.ItemFields.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public static UUID` | `attackDamageModifierID()` - Returns `Item#ATTACK_DAMAGE_MODIFIER_ID.` |
| `public static UUID` | `attackSpeedModifierID()` - Returns `Item#ATTACK_SPEED_MODIFIER_ID.` |

- ##### item.ItemHelper.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `default void` | `onStackCreated(ItemStack itemStack, int count)` - Fired on the constructor for itemstacks. All items are automatically an instance of `ItemHelper`, so checks should be made when using this to avoid running unnecessary logic on all itemstack creation events. Example usage includes attaching nbt data when an itemstack is first created. |
| `default float` | `getAttackDamage(ItemStack itemStack)` - Returns `0.0` by default. ItemStack dependent version of `SwordItem#getAttackDamage` and `MiningToolItem#getAttackDamage;` for the `SwordItem` and `MiningToolItem`, returns the non-itemstack dependent methods. |
| `default float` | `getProtection(ItemStack itemStack)` - Returns `0.0` by default. `ItemStack` dependent version of `ArmorItem#getProtection`; for `ArmorItem`, returns the non-itemstack dependent method. |
| `default float` | `getToughness(ItemStack itemStack)` - Returns `0.0` by default. `ItemStack` dependent version of `ArmorItem#getToughness;` for `ArmorItem`, returns the non-itemstack dependent method. |
| `SoundEvent` | `getEquipSound(ItemStack itemStack)` - Returns `null` by default. `ItemStack` dependent version of `Item#getEquipSound`. |

- ##### util.Maths.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public static Map<K, V>` | `enumLookupMap(V[] values, Function<V, K> mapping)` - Creates a static lookup map for the input `enum#values` implementation. |
| `public static float` | `parse(String string)` - Parses a string to a float. If it cannot be parsed, returns 0F. |
| `public static double` | `stairs(double x, double stretch, double steepness, double xOffset, double yOffset)` - A staircase function based on `Math#sin`. |
| `public static boolean` | `isWithinLimits(double value, double min, double max)` - Returns `true` if the value is less than max and greater than or equal to min. |

- ##### util.RandDistribution.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public` | `RandDistribution(T fallback)` - Constructor. |
| `public void` | `add(T input, float weight)` - Adds to the internal inventory of the distributor. |
| `public T` | `getDistributedRandom()` - Returns a randomly distributed and weighted result. |

- ##### util.VoidConsumer.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `void` | `accept()` - A functional interface equivalent to a Consumer, but with no parameters. |

- ##### DataAttributesAPI.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public static final String` | `MODID` - The mod id. |
| `public static Supplier<EntityAttribute>` | `getAttribute(Identifier registryKey)` - Returns a `Supplier` getting the registered attribute assigned to the input key. Uses a supplier because attributes added through json are null until datapacks are loaded/synced to the client, so static initialisation would not work. |
| `public static T` | `ifPresent(LivingEntity entity, Supplier<EntityAttribute> attribute, T fallback, Function<Float, T> function)` - Allows for Optional-like use of attributes that may or may not exist all the time. This is the correct way of getting and using values from attributes loaded by datapacks. If the entity is not null and the input attribute exists, and the entity has the input attribute, passes and returns the input function. Otherwise, returns the fallback input. |