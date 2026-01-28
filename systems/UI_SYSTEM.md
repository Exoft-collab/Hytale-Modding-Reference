# Hytale UI System Reference

Hytale uses a hybrid UI system combining XML-like markup for layout and Java for logic.

## 1. UI Markup (.ui files)
- **Location**: Typically in `src/main/resources/Common/UI/Custom/Pages/`.
- **Syntax**: Uses a proprietary format starting with `$C = "../Common.ui";`.
- **Selectors**: 
    - `#ID`: Direct reference to an element (used in Java via `cmd.set("#ID...", value)`).
    - `@Property`: Overrides/defines a property (e.g., `@Text`, `@Anchor`).
- **Nesting**: Elements like `Group`, `Button`, `Label`, `Image`, `Container` are common.
- **Dynamic Insertion**: Elements can be appended to slots (e.g., `cmd.append("#SlotID", "Path/To/Entry.ui")`).

## 2. Java Integration (Pages)
- **Base Class**: `InteractiveCustomUIPage<T>` where `T` is an EventData class.
- **Lifetime**: Handled by `CustomPageLifetime` (e.g., `CanDismissOrCloseThroughInteraction`).
- **Codec**: Requires a `BuilderCodec` to map data between the client and server.

## 3. Communication Tools
- `UICommandBuilder`: Used in the `build()` method to send initial state to the client (e.g., `cmd.set("#Title.Text", "Hello")`).
- `UIEventBuilder`: Used to bind client-side interactions (e.g., `CustomUIEventBindingType.Activating`) to server-side actions.
- `handleDataEvent`: The entry point for all client-to-server interactions.

## 4. Useful Components
- `TextField`: For user input (e.g., `#Input.Value`).
- `TooltipTextSpans`: For rich-text tooltips using the `Message` class.
- `Style` nested objects: For modifying colors, font sizes, and alignments dynamically.
