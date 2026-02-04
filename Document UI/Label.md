# Label

Standard element for displaying text.

## Confirmed Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| `Text` | String | The content to display. |
| `Style` | Block | Text styling (Font, Color, etc.). |
| `Anchor` | Block | Dimensions and positioning. |
| `Padding` | Block | Inner spacing. |
| `FlexWeight` | Number | Dynamic layout weight. |
| `Background` | Block | Background color `(Color: #Hex)`. |
| `OutlineColor`| Color | Border color. |
| `OutlineSize` | Number | Border thickness. |
| `ContentHeight`| Number | Internal height adjustment. |

## Confirmed Style Attributes

| Attribute | Type | Values |
| :--- | :--- | :--- |
| `FontSize` | Number | Size of the text. |
| `TextColor` | Color | Hex/RGBA color. |
| `RenderBold` | Boolean | Bold formatting. |
| `RenderUppercase`| Boolean | Uppercase formatting. |
| `LetterSpacing` | Number | Space between characters. |
| `HorizontalAlignment` | Enum | `Left`, `Center`, `Right`, `End` |
| `VerticalAlignment` | Enum | `Top`, `Center`, `Bottom`, `End` |

## Comprehensive Example

```hytale
Label #TitleText {
  Text: "Hytale UI Documented";
  Style: (
    FontSize: 16,
    TextColor: #ffffff,
    RenderBold: true,
    RenderUppercase: true,
    HorizontalAlignment: Center,
    VerticalAlignment: Center,
    LetterSpacing: 1.0
  );
  Anchor: (Width: 200, Height: 30);
  Padding: (Left: 5);
  FlexWeight: 1;
}
```
