# Hytale Core API Imports Reference

To develop Hytale plugins, the following package categories are essential. 

## 1. UI and Interactive Pages
Used for creating custom menus, dashboards, and input forms.
- `com.hypixel.hytale.server.core.entity.entities.player.pages.InteractiveCustomUIPage`: Base class for all custom interactive UIs.
- `com.hypixel.hytale.server.core.ui.builder.UICommandBuilder`: Updates elements on an open UI.
- `com.hypixel.hytale.server.core.ui.builder.UIEventBuilder`: Binds client-side events to server-side code.
- `com.hypixel.hytale.protocol.packets.interface_.CustomPageLifetime`: Defines how/when a page can be closed.
- `com.hypixel.hytale.protocol.packets.interface_.CustomUIEventBindingType`: Enum for event types (Activating, ValueChanged, etc.).

## 2. Universe and Players
Used for finding players, managing entities, and accessing the game world.
- `com.hypixel.hytale.server.core.universe.Universe`: The entry point for the entire game session.
- `com.hypixel.hytale.server.core.universe.PlayerRef`: Lightweight player reference.
- `com.hypixel.hytale.server.core.entity.entities.Player`: The actual player entity class (requires world thread access).
- `com.hypixel.hytale.server.core.universe.world.storage.EntityStore`: The storage layer for entity data.

## 3. Data Handling (Codec System)
Essential for passing data between the server and the custom UI client.
- `com.hypixel.hytale.codec.Codec`: Base interface for data encoding.
- `com.hypixel.hytale.codec.KeyedCodec`: Used for identifying specific data fields in a stream.
- `com.hypixel.hytale.codec.builder.BuilderCodec`: Used to build complex data objects that the UI can understand.

## 4. Commands and Permissions
Used for creating chat-based tools.
- `com.hypixel.hytale.server.core.command.system.basecommands.CommandBase`: The root class for all commands.
- `com.hypixel.hytale.server.core.command.system.CommandContext`: Holds all data about the command run.
- `com.hypixel.hytale.server.core.permissions.PermissionsModule`: Access point for the permission system.

## 5. Assets and Registry
- `com.hypixel.hytale.server.core.assets.Item`: The core item class.
- `com.hypixel.hytale.server.core.assets.DefaultAssetMap`: Used for caching loaded resources.
- `com.hypixel.hytale.server.core.plugin.PluginBase`: The main entry class for any Hytale plugin.
