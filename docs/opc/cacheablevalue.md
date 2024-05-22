---
title: Cacheable Value
project: opc
---

!!! abstract ""
    This section details the `content` and `implementation` of the `Cacheable Value interface` from a developer perspective.

The `Cacheable Value` is a type of `key-wrapper` hybrid. It serves to hold a generic type and provide a value of this type through a key-function map.

- #### Structure

The `CacheableValue` interface accepts a generic argument that represents its data type. For example, `CacheableValue<Integer>` holds an integer value. The interface has the following methods, where `V` denotes its generic type:

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `V` | **get**(**ServerPlayerEntity** player) When the player is online, gets the value from the player. When the player disconnects, this is used to cache the value on the server. |
| `V` | **readFromNbt**(**NbtCompound** tag) Reads the value from nbt. |
| `void` | **writeToNbt**(**NbtCompound** tag, **Object** value) Writes the value to nbt. |
| `Identifier` | **id**() Returns this value's 'key': should be in the form `modid:key`. Example: `opc:current_health`. |

- #### Implementation Example

For every value or piece of data that a mod wishes to cache, they must create a new `CacheableValue` implementation. The following is an example:

```java
import com.github.clevernucleus.opc.api.CacheableValue;

import net.minecraft.nbt.NbtCompound;
import net.minecraft.server.network.ServerPlayerEntity;
import net.minecraft.util.Identifier;

public class CurrentHealthValue implements CacheableValue<Float> {
    private final Identifier id;

    public CurrentHealthValue() {
        this.id = new Identifier("opc:current_health");
    }

    @Override
    public Float get(ServerPlayerEntity player) {
        return (float)player.getHealth();
    }

    @Override
    public Float readFromNbt(NbtCompound tag) {
        return tag.getFloat("CurrentHealth");
    }

    @Override
    public void writeToNbt(NbtCompound tag, Object value) {
        tag.putFloat("CurrentHealth", (Float)value);
    }

    @Override
    public Identifier id() {
        return this.id;
    }

    @Override
    public boolean equals(Object obj) {
        if(this == obj) return true;
        if(obj == null) return false;
        if(!(obj instanceof CurrentHealthValue)) return false;
        
        CurrentHealthValue currentHealthValue = (CurrentHealthValue)obj;
        return this.id.equals(currentHealthValue.id);
    }
    
    @Override
    public int hashCode() {
        return this.id.hashCode();
    }
    
    @Override
    public String toString() {
        return this.id.toString();
    }
}
```

- #### Notes

- It is important that a valid `#equals` method is also implemented, as this is used to differentiate between cacheable values. Note that simply calling `this.id#equals` is not okay here, as this checks that the input is of the type `Identifier`, which it is not.

- This implementation will cache players' current health when they disconnect, and provide this value if they are offline, or the *current* current health if they are online.

- You should try to ensure that the appropriate type is used in your generic. In the example above, `Float` is used because the return type of `player#getHealth` is `float`. Using `Integer` here would not be appropriate as you would lose data, and using `Double` here would also not be appropriate because the extra precision is not utilised.