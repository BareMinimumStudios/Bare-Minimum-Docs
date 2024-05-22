---
title: API
project: opc
---

- #### Structure & Overview

Offline Player Cache offers a small API package, located at `com.github.clevernucleus.opc.api` and contains the following:

```
📂api
 ┣📄CacheableValue.java interface
 ┗📄OfflinePlayerCache.java interface
```
The contents of `CacheableValue.java` are available [here](../opc/cacheablevalue.md/#structure). The contents of `OfflinePlayerCache.java` are shown below:

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| `public`static`final`String` | **MODID** The mod id. |
| `public`static`CacheableValue<V>` | **register**(**CacheableValue&lt;V&gt;** key)  Registers a cacheable value to the server: these are keys that instruct the server to cache some data from the players when they disconnect. |
| `public`static`T` | **getOfflinePlayerCache**(**MinecraftServer** server, **T** fallback, **Function&lt;OfflinePlayerCache, T&gt;** function)  Gains access to the offline player cache object. This should only be used on the logical server. |
| `V` | **get**(**UUID** uuid, **CacheableValue&lt;V&gt;** key)  If the player is offline and exists in the cache, retrieves the last cached value. If the player is online, retrieves the player's current value. |
| `V` | **get**(**String** name, **CacheableValue&lt;V&gt;** key)  If the player is offline and exists in the cache, retrieves the last cached value. If the player is online, retrieves the player's current value. |
| `Collection<UUID>` | **playerIds**()  Returns all offline/cached AND online players' UUIDs. |
| `Collection<String>` | **playerNames**()  Returns all offline/cached AND online players' names. |
| `boolean` | **isPlayerCached**(**UUID** uuid)  Tests if the player with the input UUID exists in the cache. |
| `boolean` | **isPlayerCached**(**String** name)  Tests if the player with the input name exists in the cache. |

- #### Utilizing Offline Player Cache

This section will use the cacheable value implementation shown [here](../opc/cacheablevalue.md/#implementation-example) to further provide an example of how OPC could be used in a project:

- #### Registering a Cacheable Value

```java
import com.github.clevernucleus.opc.api.OfflinePlayerCache;

import net.fabricmc.api.ModInitializer;

public class ExampleMod implements ModInitializer {

    // Register our CurrentHealthValue from before.
    public static final CacheableValue<Float> CURRENT_HEALTH_VALUE = OfflinePlayerCache
    .register(new CurrentHealthValue());

    @Override
    public void onInitialize() {}
}
```

That is all that is needed. Players will disconnect and their current health will be cached on the server, ready to be accessed at any time. An potential usage is with commands - in the next example, we create a new command: `/example health <name>`.

- #### Creating a Command with OPC

```java
public static void register(CommandDispatcher<ServerCommandSource> dispatcher) {
    LiteralCommandNode<ServerCommandSource> exampleNode = CommandManager
    .literal("example").build();
    dispatcher.getRoot().addChild(exampleNode);
    
    LiteralCommandNode<ServerCommandSource> currentHealthNode = CommandManager
    .literal("health").build();
    exampleNode.addChild(currentHealthNode);

    ArgumentCommandNode<ServerCommandSource, String> nameNode = CommandManager
    .argument("name", StringArgumentType.string()).executes(ctx -> {
        MinecraftServer server = ctx.getSource().getServer();

        return OfflinePlayerCache.getOfflinePlayerCache(server, -1, opc -> {
            String player = StringArgumentType.getString(ctx, "name");
            float health = opc.get(player, ExampleMod.CURRENT_HEALTH_VALUE);
            LiteralText msg = new LiteralText(player + "'s current health is: " + health);

            ctx.getSource.sendFeedback(msg, false);

            return 1;
        });
    }).build();
    currentHealthNode.addChild(nameNode);
}
```
Using examples [A](../opc/api.md/#registering-a-cacheable-value) and [B](../opc/api.md/#creating-a-command-with-opc), we have added the command `/example health <name>`. 

The output of which is `<name>'s health is: <health>`. You can now effectively get the current health of all online and offline players on your server. 

This is obviously a very simplistic example: usually you would also add a `command node` to take a `UUID argument` in addition to the `player's name`; and you would have a `suggestion provider` to give you all player names/uuids. 