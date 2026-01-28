# Example: Complex UI with Lists & Input

This example creates a "Staff List" UI where you can filter by name.
It demonstrates:
1.  Dynamic Slot Loading (for lists).
2.  Input Handling (TextField).
3.  Event Binding.

### 1. The Layout (`StaffPage.ui`)
```c
$C = "../Common.ui";

$C.@PageOverlay {
    $C.@DecoratedContainer {
        Anchor: (Width: 600, Height: 500);
        
        // Header
        Label { Text: "Staff Directory"; Anchor: (Top: 20, Height: 30); }
        
        // Search Input
        $C.@TextField #SearchInput {
            @Anchor: (Top: 60, Height: 40, Width: 400);
            PlaceholderText: "Filter by name...";
        }
        
        // Search Button
        $C.@StyledButton #SearchButton {
            @Text: "Search";
            @Anchor: (Top: 60, Right: 50, Height: 40, Width: 100);
        }
        
        // List Container
        $C.@ScrollContainer #ListContainer {
            @Anchor: (Top: 120, Bottom: 20, Left: 20, Right: 20);
            LayoutMode: Top; 
            // We will inject slots here
        }
    }
}
```

### 2. The Item Template (`StaffEntry.ui`)
```c
$C = "../Common.ui";

Group {
    Anchor: (Height: 50, Full: 0); // Full width
    Background: #333333;
    
    Label #NameLabel {
        Text: "Player Name";
        Style: (TextColor: #FFFFFF, VerticalAlignment: Center);
    }
}
```

### 3. The Java Controller

```java
public class StaffPage extends InteractiveCustomUIPage<StaffPage.StaffEvent> {

    // Helper Codec Class
    public static class StaffEvent {
        public String action;
        public String searchTerm; // Bound to #SearchInput
        
        public static final BuilderCodec<StaffEvent> CODEC = BuilderCodec.builder(StaffEvent.class, StaffEvent::new)
             .addField(new KeyedCodec<>("action", Codec.STRING), (o,v)->o.action=v, o->o.action)
             // Bind @SearchInput value to 'searchTerm'
             .addField(new KeyedCodec<>("@SearchInput", Codec.STRING), (o,v)->o.searchTerm=v, o->o.searchTerm)
             .build();
    }
    
    private List<String> staffNames = Arrays.asList("Sabo", "Admin1", "Mod2", "Helper3");

    public StaffPage() {
        super(StaffEvent.CODEC); // Register Codec
    }

    @Override
    public void build(Ref<EntityStore> ref, UICommandBuilder cmd, UIEventBuilder event, Store<EntityStore> s) {
        cmd.append("Pages/StaffPage.ui");
        
        // Initial List
        renderList(cmd, "");
        
        // Bind Button
        event.addEventBinding(CustomUIEventBindingType.Activating, "#SearchButton",
             new StaffEvent().append("action", "SEARCH"));
    }
    
    private void renderList(UICommandBuilder cmd, String filter) {
        // Clear previous? (Hytale UI logic might require removing old slots first or rebuilding)
        // For simplicity, let's assume we just append valid ones.
        // In reality, you'd likely rebuild the container or use a paged system.
        
        int slotId = 0;
        for (String name : staffNames) {
            if (name.toLowerCase().contains(filter.toLowerCase())) {
                String slot = "#Slot" + slotId;
                
                // inject entry into #ListContainer
                cmd.append(slot, "Pages/StaffEntry.ui", "#ListContainer"); 
                
                // Update Text
                cmd.set(slot + " #NameLabel.Text", name);
                
                slotId++;
            }
        }
    }

    @Override
    public void handleDataEvent(Ref<EntityStore> ref, Store<EntityStore> store, StaffEvent data) {
        if ("SEARCH".equals(data.action)) {
            // Re-render
            UICommandBuilder cmd = new UICommandBuilder();
            
            // "Clear" trick: often involves removing elements or just updating existing ones.
            // A common pattern is fully closing & reopening for complex list changes, 
            // OR hiding unused slots.
            
            // For this example, let's just log it
            System.out.println("Searching for: " + data.searchTerm);
            
            // In a real app, you would efficiently update the list here.
        }
    }
}
```
