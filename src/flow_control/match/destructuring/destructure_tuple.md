# タプル

タプルは`match`内で以下のように分割代入できます。

```rust,editable
fn main() {
    let pair = (0, -2);
    // TODO ^ `pair`を違う値にして試してみてください

    println!("Tell me about {:?}", pair);  // {:?}について教えて
    // matchはタプルの分割代入に使えます。
    match pair {
        // 2つ目を分割代入する
        (0, y) => println!("First is `0` and `y` is `{:?}`", y),  // 最初は`0`で`y`は`{:?}`です
        (x, 0) => println!("`x` is `{:?}` and last is `0`", x),  // `x`は`{:?}`で最後は`0`です
        _      => println!("It doesn't matter what they are"),  // これが何なのかわかりません
        // `_` は値を変数として束縛しないことを意味します。
    }
}
```

### こちらも参照:

[タプル](../../../primitives/tuples.md)
