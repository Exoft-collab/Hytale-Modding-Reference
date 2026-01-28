# Example: Async Database to World Thread

This is the **most critical** pattern to learn. It prevents server freezes.
Scenario: A player requests their stats. We must fetch from a slow DB/File, then show it in chat.

```java
package com.example.myplugin;

import com.hypixel.hytale.server.core.entity.entities.Player;
import com.hypixel.hytale.server.core.universe.Universe;
import com.hypixel.hytale.server.core.universe.PlayerRef;
import java.util.UUID;
import java.util.concurrent.CompletableFuture;

public class StatsManager {

    // Simulating a database call
    private String fetchStatsFromDatabase(UUID uuid) {
        try {
            // Simulate 500ms network lag
            Thread.sleep(500); 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "{Kills: 10, Deaths: 2} for " + uuid.toString();
    }

    public void showStatsToPlayer(UUID playerUUID) {
        
        // 1. ASYNC STEP: Run heavy task off the main thread
        CompletableFuture.runAsync(() -> {
            
            // We are now in a generic worker thread. 
            // DO NOT touch 'Player' object here.
            
            String statsData = fetchStatsFromDatabase(playerUUID);
            
            // 2. RETRIEVE REVERENCE: Get a thread-safe handle
            PlayerRef ref = Universe.get().getPlayer(playerUUID);
            
            if (ref != null && ref.isValid()) {
                
                // 3. SYNC STEP: Schedule logic back to World Thread
                
                // Note: ref.getEntity() *might* be unsafe if called directly here?
                // Actually, accessing the World from the Ref then executing is standard.
                // Or better:
                
                // We resolve the entity inside the execute block to be 100% safe.
                
                Universe.get().getWorld().execute(() -> {
                     
                     // We are back on the World Thread!
                     if (ref.isValid() && ref.isOnline()) {
                         // Safe to get Component now
                         Player player = ref.get().getComponent(Player.getComponentType());
                         
                         if (player != null) {
                             player.sendMessage("Your Stats: " + statsData);
                         }
                     }
                });
            }
        });
    }
}
```

### Why this works:
1.  `runAsync`: Moves `Thread.sleep` off the main loop. Server TPS stays at 20.
2.  `Universe.get().getPlayer`: Safe to call anywhere (read-only lookup).
3.  `execute()`: The specific World queue runs this lambda at the start of the next Tick.
4.  `getComponent`: Only called inside that tick.
