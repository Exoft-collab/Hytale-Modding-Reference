# DropdownBox

Selection menu with optional search.

## Confirmed Properties

- `ShowSearchInput`: Boolean - Toggle search bar.
- `Style`: Block - Styling for the box and search field.
- `Anchor`: Block - Dimensions and positioning.

## SearchInputStyle Attributes

| Key | Description |
| :--- | :--- |
| `PlaceholderText` | Text when empty. |
| `PlaceholderStyle`| Style for placeholder. |
| `Icon` | Inner icon block (`Texture`, `Width`, `Height`, `Offset`). |
| `ClearButtonStyle`| Style for 'X' button. |
| `Anchor` / `Padding`| Placement inside the dropdown. |

## Comprehensive Example

```hytale
$C.@DropdownBox #Selector {
  ShowSearchInput: true;
  Style: (
    SearchInputStyle: (
      PlaceholderText: "Search...",
      Icon: (Texture: "Icons/Search.png", Width: 16, Height: 16, Offset: 5)
    )
  );
  Anchor: (Width: 200, Height: 30);
}
```
