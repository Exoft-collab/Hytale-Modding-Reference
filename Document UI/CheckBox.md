# CheckBox

On/Off toggle with label.

## Confirmed Properties

- `@Text`: String - Label text.
- `@Checked`: Boolean - Initial state.
- `Padding`: Block - Spacing.
- `Anchor`: Block - Positioning.

## Comprehensive Example

```hytale
$C.@CheckBoxWithLabel #MyToggle {
  @Text = "Enable Setting";
  @Checked = true;
  Padding: (Bottom: 10);
}
```
