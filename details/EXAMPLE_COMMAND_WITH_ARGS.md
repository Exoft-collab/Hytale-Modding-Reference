# Example: Command with Arguments & Parsing

Scenario: A `/pay` command: `/pay <player> <amount>`

```java
package com.example.myplugin;

import com.hypixel.hytale.server.core.command.system.basecommands.CommandBase;
import com.hypixel.hytale.server.core.command.system.CommandContext;
import com.hypixel.hytale.server.core.universe.Universe;
import com.hypixel.hytale.server.core.universe.PlayerRef;
// ... imports

public class PayCommand extends CommandBase {

    public PayCommand() {
        super("pay", "Send money to players", false);
        this.setAllowsExtraArguments(true); // REQUIRED for args
    }

    @Override
    protected void executeSync(CommandContext ctx) {
        if (!ctx.isPlayer()) {
            ctx.sender().sendMessage("Players only!");
            return;
        }

        // 1. Parse Input
        String raw = ctx.getInputString(); // "Sabo 100"
        if (raw == null || raw.isEmpty()) {
            sendHelp(ctx);
            return;
        }

        String[] parts = raw.trim().split("\\s+");
        if (parts.length < 2) {
            sendHelp(ctx);
            return;
        }

        String targetName = parts[0];
        String amountStr = parts[1];

        // 2. Resolve Player
        PlayerRef targetRef = Universe.get().getPlayer(targetName);
        if (targetRef == null || !targetRef.isValid()) {
             ctx.sender().sendMessage("Player '" + targetName + "' not found!");
             return;
        }

        // 3. Parse Amount
        int amount;
        try {
            amount = Integer.parseInt(amountStr);
        } catch (NumberFormatException e) {
             ctx.sender().sendMessage("Invalid amount!");
             return;
        }

        if (amount <= 0) {
             ctx.sender().sendMessage("Amount must be positive!");
             return;
        }

        // 4. Logic (Simplified)
        UUID senderUUID = ctx.sender().getUuid();
        UUID targetUUID = targetRef.get().getUuid();

        // Perform transaction...
        ctx.sender().sendMessage("You sent $" + amount + " to " + targetName);
        
        // Notify target if online
        if (targetRef.isOnline()) {
             // We need to resolve the target Component to message them
             // Since we are in executeSync, we are on the WorldThread.
             // We can safely access the target's request context if in same world, 
             // but 'PlayerRef' notification helper is safer.
             
             // Or safely:
             targetRef.get().sendMessage("You received $" + amount);
        }
    }

    private void sendHelp(CommandContext ctx) {
        ctx.sender().sendMessage("Usage: /pay <player> <amount>");
    }
}
```
