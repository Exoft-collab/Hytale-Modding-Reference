# Advanced Styles & Template Blocks

These are specialized style definitions and templates. They are **not** applicable to every element; each has a specific usage context.

## 1. PatchStyle (9-Slice Scaling)
**Usage:** Applicable to **Background** or **Outline** properties of any element (Group, Button, TextField, etc.).
Allows a texture to scale without distorting its corners.

| Parameter | Function |
| :--- | :--- |
| `TexturePath` | Path to the texture image. |
| `Border` | Corner size in pixels that stays fixed during scaling. |

**Example (TextField Background):**
```hytale
$C.@TextField #Search {
  Background: PatchStyle(TexturePath: "Common/SearchBg.png", Border: 4);
}
```

## 2. ButtonStyle
**Usage:** Specifically for the **Style** block of a **Button** or **TextButton**.
Defines appearance changes based on the button's state.

| State | Context |
| :--- | :--- |
| `Default` | Normal state. |
| `Hovered` | Mouse cursor is over the button. |
| `Pressed` | Button is currently being clicked. |
| `Disabled`| Button interaction is disabled. |

**Example:**
```hytale
Style: ButtonStyle(
  Default: (Background: #203550),
  Hovered: (Background: #28a745)
)
```

## 3. LabelStyle
**Usage:** Specifically for defining reusable **Label** styles.
Found in `Style` blocks of Labels or as part of a `ButtonStyle`'s states for TextButtons.

**Example:**
```hytale
@MyLabelStyle = LabelStyle(
  FontSize: 14,
  TextColor: #bfcdd5,
  RenderBold: true
);

Label {
  Style: @MyLabelStyle;
}
```

## 4. Custom UI Templates (from Common.ui)
These are specific pre-configured blocks provided by the project's base library (`$C`).

- `$C.@PageOverlay`: A base container for full-screen UI.
- `$C.@DecoratedContainer`: A window with a title bar (`#Title`) and content area (`#Content`).
- `$C.@Title`: A standardized header label.
- `$C.@CloseButton`: A standardized exit button.
