# Hytale Modding Architecture: 01 - Fundamentals

**Purpose:** This document outlines the entry point, lifecycle, and core dependency injection patterns observed in Hytale mod development.

## 1. The Plugin Entry Point
Every Hytale server plugin must extend `PluginBase`. This is the mod's "Main Class".

```java
import com.hypixel.hytale.server.core.plugin.PluginBase;

public class MyPlugin extends PluginBase {
    // Singleton Pattern (Recommended for easy access)
    private static MyPlugin instance;

    @Override
    public void onEnable() {
        instance = this;
        // Initialization logic here
    }

    @Override
    public void onDisable() {
        // Cleanup logic here
    }
}
```

## 2. Event Registration
Events are not registered via a central "EventBus" object, but rather by passing method references or lambda functions to specific managers found in the `Universe` or `Server` context.

**Example: Player Disconnect**
In `onEnable`:
```java
// Listen to player disconnects
this.getServer().getEventBus().register(
    com.hypixel.hytale.server.core.event.player.PlayerLeaveEvent.class, 
    this::onPlayerLeave
);
```

## 3. Asset Loading (Items)
Hytale items are loaded asynchronously as assets. Plugins must listen for the `LoadedAssetsEvent` to cache or manipulate items.

**Key Definition:**
`LoadedAssetsEvent<String, Item, DefaultAssetMap<String, Item>>`

**Implementation Pattern:**
1. Create a `static Map<String, Item> ITEMS` cache.
2. In `onEnable`, register the listener.
3. In the listener method, populate the map.

```java
private void onItemAssetLoad(LoadedAssetsEvent<String, Item, DefaultAssetMap<String, Item>> event) {
    if (event.getLoadedAssets() != null) {
        // "Generic Item" -> ItemObject
        MY_ITEM_CACHE.putAll(event.getLoadedAssets()); 
    }
}
```

## 4. Logging
Hytale provides a scoped logger for plugins.
```java
// Logger usage
LOGGER.atInfo().log("Plugin Enabled!");
LOGGER.atError().log("Something went wrong", exception);
```
