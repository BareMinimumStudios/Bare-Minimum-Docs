---
title: In-game Content
project: playerex
---

!!! abstract ""
    `PlayerEx` adds new `attributes` to the game, that are by default attached to the player. Additionally, PlayerEx features a new `GUI` screen and `levelling system.`


- #### Attributes

 PlayerEx adds fully functional attributes to the game. By default these attributes are only present on the player, but attributes with the **Applicable Entity** type `LivingEntity` can be attached to any living entity with datapacks - see [Data Attributes](../data-attributes/datapack-setup.md).


| Attribute Registry Key | Applicable Entity | Stacking Behaviour | Description |
| -------- | --------------- | ---------------- | ---------- |
| Evasion`playerex:evasion`| `LivingEntity` | `DIMINISHING` | The chance to dodge projectiles. |
| Lifesteal`playerex:lifesteal` | `LivingEntity` | `DIMINISHING` | A percentage of melee and ranged damage dealth is healed. |
| Health Regeneration`playerex:health_regeneration` | `LivingEntity` | `DIMINISHING` | Health passively healed every second. |
| Heal Amplification`playerex:heal_amplification` | `LivingEntity` | `DIMINISHING` | All healing is amplified by this amount. |
| Melee Crit Damage`playerex:melee_crit_damage` | `PlayerEntity` | `DIMINISHING` | Attack damage is multiplied by this amount on a melee critical hit. |
| Melee Crit Chance`playerex:melee_crit_chance` | `PlayerEntity` | `DIMINISHING` | The chance for a melee attack to be a critical hit. |
| Ranged Crit Damage`playerex:ranged_crit_damage` | `LivingEntity`| `DIMINISHING` | Projectile damage is multiplied by this amount on a rangd critical hit. |
| Ranged Crit Chance`playerex:ranged_crit_chance` | `LivingEntity`| `DIMINISHING` | The chance for a projectile fired to result in a critical hit. |
| Ranged Bonus Damage`playerex:ranged_damage` | `LivingEntity` | `FLAT` | Damage added to projectile base damage. |
| Fire Resistance`playerex:fire_resistance` | `LivingEntity` | `DIMINISHING` | Reduces fire damage by this amount (100% is immunity). |
| Freeze Resistance`playerex:freeze_resistance` | `LivingEntity` | `DIMINISHING` | Reduces freeze damage by this amount (100% is immunity). |
| Lightning Resistance`playerex:lightning_resistance` | `LivingEntity` | `DIMINISHING` | Reduces lightning damage by this amount (100% is immunity). |
| Poison Resistance`playerex:poison_resistance` | `LivingEntity` | `DIMINISHING` | Reduces poison damage by this amount (100% is immunity). |
| Wither Resistance`playerex:wither_resistance` | `LivingEntity` | `DIMINISHING` | Reduces wither damage by this amount (100% is immunity). |
| Breaking Speed`playerex:breaking_speed` | `PlayerEntity` | `FLAT` | Defines the player's base block breaking speed. |
| Level`playerex:level` | `LivingEntity` | `FLAT` | An RPG like level value attribute; does nothing. |
| Constitution`playerex:constitution` | `LivingEntity` | `FLAT` | An RPG like stat value used to increase other attributes: +1 Max Health +0.1 Knockback Resistance +0.1 Poison Resistance |
| Strength`playerex:strength` | `LivingEntity` | `FLAT` | An RPG like stat value used to increase other attributes: +0.25 Melee Attack Damage +0.5 Armor +0.01 Health Regen./s |
| Dexterity`playerex:dexterity` | `LivingEntity` | `FLAT` | An RPG like stat value used to increase other attributes: +0.1 Attack Speed +0.25 Ranged Damage +5% Melee Crit Damage +0.1 Lightning Resistance |
| Intelligence`playerex:intelligence` | `LivingEntity` | `FLAT` | An RPG like stat value used to increase other attributes: +2% Heal Amplification +5% Ranged Crit Damage +0.1 Wither Resistance |
| Luckiness`playerex:luckiness` | `LivingEntity` | `FLAT` | An RPG like stat value used to increase other attributes: +0.1 Luck +2% Evasion +2% Melee Crit Chance +2% Ranged Crit Chance |
