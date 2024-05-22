---
title: Text Placeholder API
project: playerex
---

!!! abstract ""
    Since version `3.0.2`, PlayerEx supports `Text Placeholder API` and includes three text placeholders.

- #### Provided Placeholders

| Placeholder | Online Status | Result |
| ---------- | ------------ | ----- |
| `%playerex:level%` | Online Players Only | This displays the player's current level attribute value. |
| `%playerex:name_top/x%` | Offline and Online Players Only | This displays the player's name whose level is the highest in the current world; where `x` represents the relative position of the player in terms of level. For example: for a player with the highest level, `x` would be 1; for the player with the second highest level, `x` would be 2, and so on. The name displayed is provided by `GameProfile#getName`. |
| `%playerex:level_top/x%` | Offline and Online Players Only | This displays the player's level whose level is the highest in the current world; where `x` represents the relative position of the player in terms of level. For example: for a player with the highest level, `x` would be 1; for the player with the second highest level, `x` would be 2, and so on. The value displayed is provided by `LivingEntity#getAttributeValue`. |

- #### Notes

When a player leaves a world `(disconnects)`, PlayerEx caches that player's `UUID`, `Name` and `Level`. These values are used by the server when providing placeholders for offline players, whereas for online players the current/life value is provided instead of the cached one. Upon joining a world (reconnecting) that player's cache is removed and their current/live values are used once again.

This offline caching functionality is provided by [Offline Player Cache](../opc/home.md) - a lighweight library/API mod. PlayerEx ships with it included (jar-in-jar). There may be circumstances where a player has quit a world/server but their cache is still there. To remove their cache, or even just their cached `Name` or `Level`, please see [here](../opc/home.md/#commands).

Also see [Text Placeholder API](https://github.com/Patbox/TextPlaceholderAPI)