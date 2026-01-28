# Example: Full Plugin Main Class

This example demonstrates a production-ready `PluginBase` implementation including:
1.  Singleton access.
2.  Config loading (JSON).
3.  Asset caching (Items).
4.  Event registration (Player Join).
5.  Command registration.

```java
package com.example.myplugin;

import com.hypixel.hytale.server.core.plugin.PluginBase;
import com.hypixel.hytale.server.core.event.player.PlayerJoinEvent;
import com.hypixel.hytale.server.core.event.assets.LoadedAssetsEvent;
import com.hypixel.hytale.server.core.assets.Item;
import com.hypixel.hytale.server.core.assets.DefaultAssetMap;
import org.slf4j.Logger; // Hytale uses SLF4J
import java.io.File;
import java.util.Map;
import java.util.HashMap;

public class MyPlugin extends PluginBase {

    // Singleton Instance
    private static MyPlugin instance;
    
    // Asset Cache
    public static final Map<String, Item> ITEM_CACHE = new HashMap<>();
    
    // Config Data (Simple example)
    private MyConfig config;

    @Override
    public void onEnable() {
        instance = this;
        LOGGER.info("MyPlugin is starting...");

        // 1. Load Config
        loadConfig();

        // 2. Register Event Listeners
        // Player Join
        this.getServer().getEventBus().register(PlayerJoinEvent.class, this::onPlayerJoin);
        // Asset Load
        this.getServer().getEventBus().register(LoadedAssetsEvent.class, this::onAssetsLoaded);

        // 3. Register Commands
        this.getCommandRegistry().registerCommand(new MyCommand());

        LOGGER.info("MyPlugin started successfully!");
    }

    @Override
    public void onDisable() {
        // Cleanup
        ITEM_CACHE.clear();
        instance = null;
        LOGGER.info("MyPlugin disabled.");
    }

    // --- Singleton Accessor ---
    public static MyPlugin getInstance() {
        return instance;
    }

    // --- Event Handlers ---

    private void onPlayerJoin(PlayerJoinEvent event) {
        // Safe to access player here? 
        // Yes, but if you need to allow them to fully load first, 
        // sometimes a slight delay or waiting for a specific packet is better.
        // For simple messages, this is fine.
        LOGGER.info("Player joined: " + event.getPlayer().getName());
    }

    private void onAssetsLoaded(LoadedAssetsEvent<String, Item, DefaultAssetMap<String, Item>> event) {
        if (event.getLoadedAssets() != null) {
            ITEM_CACHE.putAll(event.getLoadedAssets());
            LOGGER.info("Cached " + event.getLoadedAssets().size() + " items.");
        }
    }

    // --- Config Logic ---
    
    private void loadConfig() {
        File folder = this.getDataFolder();
        if (!folder.exists()) folder.mkdirs();
        
        File configFile = new File(folder, "config.json");
        // Implementation of JSON loading using GSON or Jackson would go here
        // For now, valid Hytale plugins often use Jackson or Hytale's internal serializers
    }
}
```
