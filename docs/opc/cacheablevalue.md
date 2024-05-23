---
title: CachedPlayerKey
project: opc
---

!!! abstract ""
    This section details the `content` and `implementation` of the `CachedPlayerKey` from a developer perspective.

The `CachedPlayerKey` is a type of `key-wrapper` hybrid. It serves to hold a generic type and provide a value of this type through a key-function map.

#### Structure

The `CachedPlayerKey` abstract class accepts a generic argument that represents its data type. For example, `CachedPlayerKey<Integer>` holds an `Integer` value. The interface has the following methods, where `V` denotes its generic type provided upon implementation of the class:

!!! Implementation

=== "Fabric"
    | Returned | Function | Description 
    | -------- | -------- | ---------- |
    | **`V?`** | **`get`** | When the player is online, gets the value from the player. When the player disconnects, this is used to cache the value on the server. This may be nullable. |
    | **`V`** | **`readFromNbt`** | Reads the value from `nbt`. |
    | **`void`** | **`writeToNbt`** | Writes a value to a `nbt`. |
    | **`Identifier`** | **`id`** | Returns the key's registered `Identifier`. An example would be `your_mod_id:your_key_name`. |

=== "Forge"
    | Returned | Function | Description 
    | -------- | -------- | ---------- |
    | **`V?`** | **`get`** | When the player is online, gets the value from the player. When the player disconnects, this is used to cache the value on the server. This may be nullable. |
    | **`V`** | **`readFromNbt`** | Reads the value from `nbt`. |
    | **`void`** | **`writeToNbt`** | Writes a value to a `nbt`. |
    | **`ResourceLocation`** | **`id`** | Returns the key's registered `ResourceLocation`. An example would be `your_mod_id:your_key_name`. |                                                       |

#### Creating & Registering Your Key

For every value or piece of data that a mod wishes to cache, they must implement the `CachedPlayerKey`.

The following implementation goes through the process in creating a theoretical `LevelKey`.

!!! Example
=== "Fabric"
    ```kotlin
    import com.bibireden.opc.api.CachedPlayerKey
    import net.minecraft.nbt.NbtCompound
    import net.minecraft.server.network.ServerPlayerEntity
    import net.minecraft.util.Identifier

    class LevelKey : CachedPlayerKey<Int>(Identifier("your-mod-id", "your-key-path")) {
        override fun get(player: ServerPlayerEntity): Int {
            // this would be up to the end user's interpretation, so for now, anything will suffice.
            return 404
        }

        override fun readFromNbt(tag: NbtCompound): Int {
            return tag.getInt("level")
        }

        override fun writeToNbt(tag: NbtCompound, value: Any) {
            // Due to some limitations, type checking cannot be provided for the second argument, but you can almost safely guarantee this value will be associated with your type-parameter.
            if (value is Int) tag.putInt("level", value)
        }
    }
    ```
=== "Forge"
    ```kotlin
    import com.bibireden.opc.api.CachedPlayerKey
    import net.minecraft.nbt.CompoundTag
    import net.minecraft.resources.ResourceLocation
    import net.minecraft.world.entity.player.Player

    class LevelKey : CachedPlayerKey<String>(ResourceLocation("your-mod-id", "your-key-path")) {
        override fun get(player: Player): String {
            // this would be up to the end user's interpretation, so for now, anything will suffice.
            return "playerex in forge when?"
        }

        override fun readFromNbt(tag: CompoundTag): String {
            return tag.getString("message")
        }

        override fun writeToNbt(tag: CompoundTag, value: Any) {
            tag.putString("message", value as String)
        }
    }
    ```

Now it's time to register your key! It is ideal you call upon the `OfflinePlayerCacheAPI`'s static `register` function in order to register the key properly.

```kotlin title="ExampleMod.kt"
val LEVEL_KEY = OfflinePlayerCacheAPI.register(LevelKey())
```

#### Notes

- If you are needing to check for equality between your derived class, you can override the `equals` method. This is left to you to implement yourself, we do not guarantee the equality of your instances as it is user-dependent on how you wish to go about that.

- You should try to ensure that the appropriate type is used in your generic. In the example above, `Float` is used because the return type of `player#getHealth` is a `Float`. Using `Integer` here would not be appropriate as you would lose data, and using `Double` here would also not be appropriate because the extra precision is not utilised.