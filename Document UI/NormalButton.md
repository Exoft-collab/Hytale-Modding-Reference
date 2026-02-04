# Normal Button (Ham Buton)

En temel buton türüdür. Genellikle slotlarda, ikon kutularında veya basit tıklama alanlarında konteyner olarak kullanılır.

## Kullanılan Özellikler

| Özellik | Tip | Açıklama |
| :--- | :--- | :--- |
| `Background` | Color / Hex | Sabit arka plan rengi veya resmi. |
| `Anchor` | Block | Boyut ve konumlandırma (Width, Height, Top vb.). |
| `Padding` | Block | İç boşluk. |
| `Visible` | Boolean | Görünürlük ayarı. |
| `Label` | Sub-element| İçine metin eklemek için kullanılan alt eleman. |

## Önemli Notlar
- Normal butonlar varsayılan olarak **Hovered** veya **Pressed** gibi durumları (state) desteklemez.
- Üzerinde yazı göstermek için mutlaka içine bir `Label { ... }` bloğu açılmalıdır.

## Örnek Kullanım

```hytale
// Basit bir slot veya işlem butonu
Button #SimpleBtn {
  Anchor: (Width: 120, Height: 40);
  Background: #203550;
  Label { 
    Text: "TAMAM"; 
    Anchor: (Full: 0); 
    Style: (
      FontSize: 14, 
      HorizontalAlignment: Center, 
      VerticalAlignment: Center,
      RenderBold: true
    ); 
  }
}
```
