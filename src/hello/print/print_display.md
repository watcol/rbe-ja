# ディスプレイ

`fmt::Debug`はコンパクトでもきれいでもありません。なので、たいていの場合は
出力をカスタマイズするのが好ましいでしょう。これは`{}`プリントマーカーを使う
[`fmt::Display`][fmt]を手動で実装することで可能です。次のように実装できます。

```rust
// (`use`で) `fmt`モジュールをインポートすることで利用可能にします。
use std::fmt;

// を`fmt::Display`実装する構造体を定義します。これは`i32`を含む
// `Structure`というタプル構造体です。
struct Structure(i32);

// `{}`マーカーを使用するには、その型のための`fmt::Display`トレイトが手動で
// 実装されている必要があります。
impl fmt::Display for Structure {
    // このトレイとは`fmt`が想定通りのものであることを要求します。
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // 必ず最初の要素が出力されるようにします。
        // `f`ストリームはオペレーションが成功したがどうかを表す`fmt::Result`
        // を返します。`write!`は`println!`にとても良く似た構文を持っていることに
        // 注目してください。
        write!(f, "{}", self.0)
    }
}
```

`fmt::Display`は`fmt::Debug`よりも簡潔に見えますが、`std`ライブラリでは
問題が生じます。曖昧な型ではどのように表示すればよいでしょうか?
例えば、`std` ライブラリがすべての`Vec<T>`に対して同じスタイルを提供する
とすれば、どのスタイルにすれば良いでしょう? 以下のどちらを選べばよいでしょうか?

* `Vec<path>`: `/:/etc:/home/username:/bin` (`:`で分割)
* `Vec<number>`: `1,2,3` (`,`で分割)

どちらも正解ではありません。あらゆる方に対して理想的なスタイルは存在しませんし、
`std`ライブラリがそれを提供しているわけではありません。`fmt::Display`は`Vec<T>`
などのジェネリックなコンテナには定義していません。このような場合は`fmt::Debug`
`fmt::Debug`を使用するべきです。

ジェネリックで*ない*ような*コンテナ*型ではこのような問題は生じませんので、
`fmt::Display`を実装できます。

```rust,editable
use std::fmt; // `fmt`をインポート

// 構造体は2つの値を持っています。出力を`Display`と比較するために
// `Debug`をderiveしています。
#[derive(Debug)]
struct MinMax(i64, i64);

// `Display`を`MinMax`に実装する。
impl fmt::Display for MinMax {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // `self.数字`でそれぞれの位置のデータを参照できます。
        write!(f, "({}, {})", self.0, self.1)
    }
}

// 比較のため、フィールド上の点に名前をつける構造体を定義しましょう。
#[derive(Debug)]
struct Point2D {
    x: f64,
    y: f64,
}

// `Display`を`Point2D`にも実装しましょう
impl fmt::Display for Point2D {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // `x`と`y`のみが明示的になるようにカスタマイズ。
        write!(f, "x: {}, y: {}", self.x, self.y)
    }
}

fn main() {
    let minmax = MinMax(0, 14);

    println!("Compare structures:");
    println!("Display: {}", minmax);
    println!("Debug: {:?}", minmax);

    let big_range =   MinMax(-300, 300);
    let small_range = MinMax(-3, 3);

    println!("The big range is {big} and the small is {small}",
             small = small_range,
             big = big_range);

    let point = Point2D { x: 3.3, y: 7.2 };

    println!("Compare points:");
    println!("Display: {}", point);
    println!("Debug: {:?}", point);

    // `Debug`と`Display`は実装されていますが、`{:b}`は、`fmt::Binary`
    // が実装されていないと使えないので、この例はエラーになります。
    // println!("What does Point2D look like in binary: {:b}?", point);
}
```

`fmt::Display`は実装されていますが、`fmt::Binary` されていないので、
使うことができません。`std::fmt`は多くのこのような[`トレイト`][traits]があり、
それぞれに独自の実装が必要です。詳細は[`std::fmt`][fmt]を参照してください。

### 演習

上の例の出力を確認して、`Point2D`構造体を参考にして、例として複素数構造体を定義
しましょう. うまくいけば、次のように出力されます。

```txt
Display: 3.3 + 7.2i
Debug: Complex { real: 3.3, imag: 7.2 }
```

### こちらも参照:

[`derive`][derive], [`std::fmt`][fmt], [マクロ][macros], [`struct`][structs],
[`trait`][traits], そして[use][use]

[derive]: ../../trait/derive.md
[fmt]: https://doc.rust-lang.org/std/fmt/
[macros]: ../../macros.md
[structs]: ../../custom_types/structs.md
[traits]: ../../trait.md
[use]: ../../mod/use.md
