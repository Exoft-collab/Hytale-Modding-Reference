# Hytale Modding Architecture: 04 - Commands & Permissions

**Purpose:** This document explains how to register commands, handle arguments, and integrate with Hytale's group-based permission system.

## 1. Command Registration
Commands are not annotated; they are registered explicitly in the `onEnable` phase.

```java
// Logic: Command Class -> Logic Handler
this.getCommandRegistry().registerCommand(new MyCommand());
```

## 2. Command Implementation
Extend `CommandBase` to create a standard chat command.

```java
public class MyCommand extends CommandBase {
    
    public MyCommand() {
        super("mycommand", "Description here", false);
        // "false" above means "Not Op Only" (requires permission setup)
    }

    @Override
    protected void executeSync(CommandContext ctx) {
        // Runs on Main Thread - Safe for Logic
        
        // 1. Check Sender
        if (!ctx.isPlayer()) return;
        
        // 2. Get Arguments
        String rawArgs = ctx.getInputString(); // "arg1 arg2"
        
        // 3. Logic
        ctx.sendMessage("You typed: " + rawArgs);
    }
}
```

## 3. Permission System
Hytale uses a hierarchical group system (`Default`, `Adventure`, `Moderator`, `Admin`).

### Standard Permission Check
Do not use string comparisons. Use the `.hasPermission()` method.

```java
@Override
public boolean hasPermission(CommandSender sender) {
    // Basic node check
    return sender.hasPermission("my.plugin.command");
}
```

### Automatic Group Registration
You can assign a command to default groups in the constructor. This makes the command usable immediately without complex config.

```java
// Allow these groups to use the command by default
this.setPermissionGroups("Admin", "Moderator");
```

### Permission Nodes vs. Op
- **Op (`*`)**: Has all permissions.
- **Node**: Granular control (e.g. `sabo.hytaletrade.admin`).
- **Best Practice**: Always define a specific permission string (const) for your command and check it.
