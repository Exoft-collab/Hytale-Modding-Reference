# NumberField

Input field for numeric data.

## Confirmed Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| `Format` | Block | Formatting constraints. |
| `Style` | Block | Text styling. |
| `Anchor` | Block | Dimensions and positioning. |
| `FlexWeight` | Number | Dynamic layout weight. |

## Format Block Attributes

| Key | Description |
| :--- | :--- |
| `MaxValue` | Max number allowed. |
| `MaxDecimalPlaces`| Digits after decimal. |
| `Step` | Incremental step (e.g. 0.01). |

## Comprehensive Example

```hytale
$C.@NumberField #PriceInput {
  Anchor: (Width: 100, Height: 30);
  Format: (
    MaxValue: 99999,
    MaxDecimalPlaces: 2,
    Step: 0.1
  );
}
```
