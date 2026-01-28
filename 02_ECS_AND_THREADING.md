# Hytale Modding Architecture: 02 - ECS & Threading

**Purpose:** This document explains the Entity Component System (ECS) architecture used by Hytale and the critical rules regarding thread safety (WorldThread).

## 1. The World Thread (CRITICAL)
Hytale is a multi-threaded server, but **all gameplay logic** (entity movement, inventory changes, UI updates) must occur on the **World Thread**.

- **Rule 1:** Never access `Player` entity methods (like `.sendMessage()` or `.getPageManager()`) from an async thread.
- **Rule 2:** Never perform blocking I/O (File reading, SQL queries) on the World Thread.

**Correct Pattern:**
```java
// 1. Perform heavy work asynchronously
CompletableFuture.runAsync(() -> {
    String data = readFileFromDisk(); // Safe heavy lifting
    
    // 2. Schedule UI update back to the World Thread
    player.getWorld().execute(() -> {
        player.sendMessage("Data Loaded: " + data); // Safe gameplay interaction
    });
});
```

## 2. Entity Component System (ECS)
Hytale does not use traditional OOP inheritance for entities (e.g., `class Player extends Entity`). Instead, it uses **Components**.

### Core Terms
- **PlayerRef**: A safe, lightweight handle to a player essentially a wrapper around a UUID. It does not contain data, only a pointer.
- **EntityStore**: The actual data container for an entity.
- **Component**: A data block (e.g., `Player` component, `Position` component).

### Accessing Player Data
To modify a player, you must "resolve" the reference to get the component.

```java
// 1. Get the Reference (Safe to hold anywhere)
PlayerRef ref = Universe.get().getPlayer(uuid);

// 2. Validate availability
if (ref != null && ref.isValid()) {
    
    // 3. Get the Store (The container)
    Ref<EntityStore> entityRef = ref.getReference();
    Store<EntityStore> store = entityRef.getStore();

    // 4. Resolve the SPECIFIC Component (e.g. Player.class)
    // The component type is technically a static getter on the class
    Player player = store.getComponent(entityRef, Player.getComponentType());
    
    if (player != null) {
        // Now you have the actual Entity logic object
        player.sendMessage("Hello!");
    }
}
```

## 3. The `Universe`
The `Universe` class is the singleton entry point for the entire server state.
- `Universe.get().getPlayer(UUID)`: Correct way to look up players.
