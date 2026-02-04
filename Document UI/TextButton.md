# TextButton ($C.@TextButton)

Gelişmiş, şablon tabanlı butondur. Menülerde, onay pencerelerinde ve interaktif kullanıcı arayüzlerinde ana etkileşim birimi olarak kullanılır.

## Kullanılan Özellikler

| Özellik | Tip | Açıklama |
| :--- | :--- | :--- |
| `Text` | String | Butonun üzerindeki yazı (Doğrudan özellik olarak yazılır). |
| `Style` | Block | Fare hareketlerine göre değişen stil (Default, Hovered, Pressed). |
| `Anchor` | Block | Boyut ve konumlandırma (Width, Height, Right vb.). |
| `Overscroll` | Boolean | Kaydırma payı/efekti (Genelde `false` set edilir). |
| `TooltipText` | String | Fare üzerine geldiğinde çıkan yardım metni. |
| `TextTooltipShowDelay` | Number | Yardım metninin çıkma gecikmesi (Saniye). |

## Stil Bileşenleri (Style Block)

`Style` bloğu içinde şu durumlar tanımlanabilir:
- `Default`: Normal görünüm.
- `Hovered`: Fare üzerindeyken görünüm.
- `Pressed`: Tıklanma anındaki görünüm.
- `Sounds`: Tıklama sesleri (`$C.@ButtonSounds`).

## Örnek Kullanım

```hytale
// Gelişmiş bir menü butonu
$C.@TextButton #ActionBtn {
  Text: "KAYDET";
  Style: (
    Default: (Background: #28a745, LabelStyle: (FontSize: 14)),
    Hovered: (Background: #218838),
    Pressed: (Background: #1e7e34),
    Sounds: $C.@ButtonSounds
  );
  Anchor: (Width: 150, Height: 45);
  Overscroll: false;
  TooltipText: "Değişiklikleri sunucuya kaydeder";
}
```
