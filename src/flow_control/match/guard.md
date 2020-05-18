# ガード

`match`ではアームをふるい分けするのに*ガード*が使えます。

```rust,editable
fn main() {
    let pair = (2, -2);
    // TODO ^ `pair`を違う値にしてみてください。

    println!("Tell me about {:?}", pair);  // {:?}についておしえて
    match pair {
        (x, y) if x == y => println!("These are twins"),  // これらは対です
        // ^ `if 条件`がガードとして働きます
        (x, y) if x + y == 0 => println!("Antimatter, kaboom!"),  // ドドーン!
        (x, _) if x % 2 == 1 => println!("The first one is odd"),  // はじめの要素が奇数です
        _ => println!("No correlation..."),  // 相関がありません
    }
}
```

### こちらも参照:

[タプル](../../primitives/tuples.md)
