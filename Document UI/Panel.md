# Panel

A simple UI container, often used for static visuals or icons. It is more lightweight than a `Group`.

## Available Properties

- `Background`: Texture/Color - Path to an icon or a hex color.
- `Visible`: Boolean - Controls visibility.
- `Anchor`: Block - Dimensions and positioning.

## Comprehensive Example

```hytale
Panel #StatusIcon {
  Background: "Icons/Checkmark.png";
  Anchor: (Width: 20, Height: 20, Right: 5);
  Visible: true;
}
```
