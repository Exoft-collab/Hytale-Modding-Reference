# Hytale Command System Reference

## 1. Registration
Commands are registered via the `CommandRegistry` provided by the server core.
```java
this.getCommandRegistry().registerCommand(new MyCommand());
```

## 2. Base Classes
- `CommandBase`: The standard base for implementing custom commands.
- **Key Methods**:
    - `executeSync(CommandContext ctx)`: Main execution logic on the world thread.
    - `hasPermission(CommandSender sender)`: Logic for access control.
    - `canGeneratePermission()`: Determines if the command should automatically create a permission node.

## 3. Context and Arguments
- `CommandContext`: Provides access to the sender, the world, and current input.
- `ctx.getInputString()`: Returns the raw string after the command name.
- `setAllowsExtraArguments(boolean)`: Allows the command to handle unparsed arguments (like numbers or player names).

## 4. Permission Groups
Standard groups include:
- `Default`: Basic players.
- `Adventure`: Players in adventure mode.
- `Admin`: Highest privilege level.
