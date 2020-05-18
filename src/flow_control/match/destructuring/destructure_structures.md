# 構造体

同様に、`struct`も以下のように分割代入できます。

```rust,editable
fn main() {
    struct Foo {
        x: (u32, u32),
        y: u32,
    }

    // 値を変更して、何が起こるか見てみましょう。
    let foo = Foo { x: (1, 2), y: 3 };

    match foo {
        Foo { x: (1, b), y } => println!("First of x is 1, b = {},  y = {} ", b, y),  // xの最初は1, b = {}, y = {}

        // 構造体を分割代入して、変数を改名することができます。
        // 順番は関係ありません
        Foo { y: 2, x: i } => println!("y is 2, i = {:?}", i),  // yは2, i = {:?}

        // いくつかの変数を無視することができます。
        Foo { y, .. } => println!("y = {}, we don't care about x", y),  // y = {}, xについてはわかりません
        // `x`に言及していないため、エラーになります。
        //Foo { y } => println!("y = {}", y),
    }
}
```

### こちらも参照:

- [構造体](../../../custom_types/structs.md)
- [refパターン](../../../scope/borrow/ref.md)
