# ColorPicker

Interactive color selection UI.

## Confirmed Properties

- `DisplayTextField`: Boolean - Show/Hide hex input.
- `Style`: Var - Reference to picker style.
- `Anchor`: Block - Positioning.
- `FlexWeight`: Number - Scaling weight.

## Comprehensive Example

```hytale
ColorPicker #Picker {
  DisplayTextField: true,
  Style: $C.@DefaultColorPickerStyle,
  Anchor: (Full: 1);
}
```
