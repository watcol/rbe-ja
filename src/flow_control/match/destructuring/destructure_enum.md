# enum

`enum`も似たように分割代入できます。

```rust,editable
// 列挙子を一つしか使わないので、警告を消すために
// `allow`が必要です。
#[allow(dead_code)]
enum Color {
    // この3つは名前だけで扱います。
    Red,
    Blue,
    Green,
    // これらは、カラーモデルと言って、`u32`タプルを併せて扱います。
    RGB(u32, u32, u32),
    HSV(u32, u32, u32),
    HSL(u32, u32, u32),
    CMY(u32, u32, u32),
    CMYK(u32, u32, u32, u32),
}

fn main() {
    let color = Color::RGB(122, 17, 40);
    // TODO ^ `color`の違う列挙子で試してみてください。

    println!("What color is it?");
    //  `enum`は`match`を使って分割代入できます。
    match color {
        Color::Red   => println!("The color is Red!"),
        Color::Blue  => println!("The color is Blue!"),
        Color::Green => println!("The color is Green!"),
        Color::RGB(r, g, b) =>
            println!("Red: {}, green: {}, and blue: {}!", r, g, b),
        Color::HSV(h, s, v) =>
            println!("Hue: {}, saturation: {}, value: {}!", h, s, v),  // 色相: {}, 彩度: {}, 明度: {}
        Color::HSL(h, s, l) =>
            println!("Hue: {}, saturation: {}, lightness: {}!", h, s, l),  // 色相: {}, 彩度: {}, 輝度: {}
        Color::CMY(c, m, y) =>
            println!("Cyan: {}, magenta: {}, yellow: {}!", c, m, y),
        Color::CMYK(c, m, y, k) =>
            println!("Cyan: {}, magenta: {}, yellow: {}, key (black): {}!",
                c, m, y, k),
        // すべての列挙子を使っったので、もう1つのアームは必要ありません。
    }
}
```

### こちらも参照:

- [`#[allow(...)]`][allow]
- [カラーモデル][color_models]
- [`enum`][enum]

[allow]: ../../../attribute/unused.md
[color_models]: https://en.wikipedia.org/wiki/Color_model
[enum]: ../../../custom_types/enum.md
