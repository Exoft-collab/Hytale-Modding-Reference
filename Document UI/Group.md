# Group

Container for layout and logical grouping.

## Confirmed Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| `LayoutMode` | Enum | Layout behavior (`Top`, `Left`, `Center`, `CenterMiddle`, `Right`, `TopScrolling`). |
| `Anchor` | Block | Dimensions and positioning. |
| `Background` | Color/Path | Hex color or path to an image. |
| `Padding` | Block | Inner spacing for children. |
| `FlexWeight` | Number | Dynamic layout weight. |
| `Visible` | Boolean | Visibility toggle. |
| `ScrollbarStyle`| Var | Style for scrolling groups. |
| `OutlineColor` | Color | Border color. |
| `OutlineSize` | Number | Border thickness. |

## Comprehensive Example

```hytale
Group #MainContent {
  LayoutMode: Top;
  Background: #121923;
  Anchor: (Full: 1);
  Padding: (Horizontal: 15, Vertical: 10);
  FlexWeight: 1;
  OutlineColor: #93844c;
  OutlineSize: 0.5;
}
```
