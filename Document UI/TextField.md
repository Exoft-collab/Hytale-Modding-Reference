# TextField

Input field for text entry.

## Confirmed Properties

- `PlaceholderText`: String - Ghost text when empty.
- `MaxLength`: Number - Character limit.
- `Style`: Block - Text styling.
- `Anchor`: Block - Positioning.
- `FlexWeight`: Number - Dynamic scaling.
- `Background`: Path/Var - Background style or texture.

## Comprehensive Example

```hytale
$C.@TextField #MyInput {
  PlaceholderText: "Username...";
  MaxLength: 16;
  Style: (FontSize: 14);
  Anchor: (Width: 200, Height: 40);
}
```
