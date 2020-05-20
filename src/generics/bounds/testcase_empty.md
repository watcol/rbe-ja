# テストケース: 空の境界

`トレイト`がもし何も機能を含んでいなくても、境界として使うことができます。
`Eq`や`Copy`は`std`ライブラリ内でのそのようなトレイトの例です。

```rust,editable
struct Cardinal;
struct BlueJay;
struct Turkey;

trait Red {}
trait Blue {}

impl Red for Cardinal {}
impl Blue for BlueJay {}

// 以下の関数はトレイト境界を設けているが、そのトレイトが空かどうか
// は関係ありません。
fn red<T: Red>(_: &T)   -> &'static str { "red" }
fn blue<T: Blue>(_: &T) -> &'static str { "blue" }

fn main() {
    let cardinal = Cardinal;
    let blue_jay = BlueJay;
    let _turkey   = Turkey;

    // 境界があるため、`red()`は`blue jay`に対して実行できない。
    println!("A cardinal is {}", red(&cardinal));
    println!("A blue jay is {}", blue(&blue_jay));
    //println!("A turkey is {}", red(&_turkey));
    // ^ TODO: この行をアンコメントしてみてください
}
```

### こちらも参照:

- [`std::cmp::Eq`][eq]
- [`std::marker::Copy`][copy]
- [`trait`][traits]

[eq]: https://doc.rust-lang.org/std/cmp/trait.Eq.html
[copy]: https://doc.rust-lang.org/std/marker/trait.Copy.html
[traits]: ../../trait.md
