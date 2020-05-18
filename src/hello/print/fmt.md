# フォーマット

文字列がどのようにフォーマットされるかは*フォーマット文字列*によって決まります。

* `format!("{}", foo)` -> `"3735928559"`
* `format!("0x{:X}", foo)` ->
  [`"0xDEADBEEF"`][deadbeef]
* `format!("0o{:o}", foo)` -> `"0o33653337357"`

同じ変数(`foo`)が`X`、`o`、*指定なし*のような様々な引数タイプ
によってフォーマットされます。

これらのフォーマットはそれぞれの引数タイプに応じたトレイトによって定義されています。
最も一般的なトレイとは`Display`で、これは引数タイプが未指定(例えば`{}`)のときに
呼び出されます。

```rust,editable
use std::fmt::{self, Formatter, Display};

struct City {
    name: &'static str,
    // 緯度
    lat: f32,
    // 経度
    lon: f32,
}

impl Display for City {
    // `f`はバッファで、ここにフォーマットした結果を書き込む必要があります。
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        let lat_c = if self.lat >= 0.0 { 'N' } else { 'S' };
        let lon_c = if self.lon >= 0.0 { 'E' } else { 'W' };

        // `write!`は`format!`のようなものですが、これはフォーマットされた文字列を
        // 第1引数に指定されたバッファに書き込みます。
        write!(f, "{}: {:.3}°{} {:.3}°{}",
               self.name, self.lat.abs(), lat_c, self.lon.abs(), lon_c)
    }
}

#[derive(Debug)]
struct Color {
    red: u8,
    green: u8,
    blue: u8,
}

fn main() {
    for city in [
        City { name: "Dublin", lat: 53.347778, lon: -6.259722 },
        City { name: "Oslo", lat: 59.95, lon: 10.75 },
        City { name: "Vancouver", lat: 49.25, lon: -123.1 },
    ].iter() {
        println!("{}", *city);
    }
    for color in [
        Color { red: 128, green: 255, blue: 90 },
        Color { red: 0, green: 3, blue: 254 },
        Color { red: 0, green: 0, blue: 0 },
    ].iter() {
        // fmt::Displayに実装を追加したら、そちらを使うように変更
        // してください。
        println!("{:?}", *color);
    }
}
```

[フォーマット用のトレイト一覧][fmt_traits]とその引数タイプ[`std::fmt`][fmt]
をドキュメントで確認できます。

### 演習
次のように出力する、上の`Color`構造体のための`fmt::Display`トレイトの実装を
してください。

```text
RGB (128, 255, 90) 0x80FF5A
RGB (0, 3, 254) 0x0003FE
RGB (0, 0, 0) 0x000000
```

詰まったら次の2つがヒントになるかもしれません。
 * [それぞれの色を2回以上出力する必要があるかもしれません][named_parameters],
 * `:02`で[2桁ゼロ埋め][fmt_width] できます。

### こちらも参照:

- [`std::fmt`][fmt]

[named_parameters]: https://doc.rust-lang.org/std/fmt/#named-parameters
[deadbeef]: https://en.wikipedia.org/wiki/Deadbeef#Magic_debug_values
[fmt]: https://doc.rust-lang.org/std/fmt/
[fmt_traits]: https://doc.rust-lang.org/std/fmt/#formatting-traits
[fmt_width]: https://doc.rust-lang.org/std/fmt/#width
