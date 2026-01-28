# Hytale Modding Architecture: 03 - UI System

**Purpose:** This document details the Hytale Custom UI system, focusing on the hybrid relationship between XML-like layouts and Java-based logic controllers.

## 1. The Structure (Split Architecture)
A custom UI consists of two mandatory parts:
1.  **Layout (`.ui` file)**: Defines the visual appearance (XML-like).
2.  **Controller (`Java` class)**: Defines the logic and data binding.

## 2. The Layout File (.ui)
Located in `src/main/resources`. Uses a hierarchical, bracket-based syntax.

**Example:**
```c
$C = "../Common.ui"; // Import common styles

$C.@PageOverlay {           // Use standard page container
    #MyButton {             // Define ID for Java access
        Text: "Click Me";
        Width: 200;
        Height: 50;
    }
}
```

## 3. The Controller Class (Java)
Must extend `InteractiveCustomUIPage<T>`.

### The `build()` Method
This method is called *once* when the UI opens. It is used to send the specific `.ui` file and initial values to the client.

```java
@Override
public void build(Ref<EntityStore> ref, UICommandBuilder cmd, UIEventBuilder event, Store<EntityStore> store) {
    // 1. Send the UI Layout
    cmd.append("Pages/MyPage.ui");

    // 2. Set dynamic values
    cmd.set("#MyButton.Text", "Dynamic Title!");

    // 3. Bind Events (Action -> ID -> EventData)
    event.addEventBinding(
        CustomUIEventBindingType.Activating, // Click event
        "#MyButton",                         // Element ID
        new EventData().append("Action", "ButtonClick") // Data to send back
    );
}
```

### The `handleDataEvent()` Method
This is the **only** way the client communicates back to the server.

```java
@Override
public void handleDataEvent(Ref<EntityStore> ref, Store<EntityStore> store, MyEventData data) {
    if ("ButtonClick".equals(data.action)) {
        // Logic when button is clicked
        System.out.println("Button was clicked!");
        
        // Update UI without closing it
        UICommandBuilder update = new UICommandBuilder();
        update.set("#MyButton.Text", "Clicked!");
        sendUpdate(update, new UIEventBuilder(), false);
    }
}
```

## 4. The Codec System (Data Binding)
For data (like "Action") to travel from Client -> Server, you must register a `Codec`.

```java
public static class MyEventData {
    public String action; // Field to hold data

    // Codec Definition
    public static final BuilderCodec<MyEventData> CODEC = BuilderCodec
        .builder(MyEventData.class, MyEventData::new)
        .addField(
            new KeyedCodec<>("Action", Codec.STRING), // JSON Key "Action"
            (obj, val) -> obj.action = val,           // Setter
            obj -> obj.action                         // Getter
        )
        .build();
}
```
