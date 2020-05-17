# 可変性

変数束縛はデフォルトで不変ですが、`mut`修飾子で上書きできます。

```rust,editable,ignore,mdbook-runnable
fn main() {
    let _immutable_binding = 1;
    let mut mutable_binding = 1;

    println!("Before mutation: {}", mutable_binding); // 変更する前: {}

    // Ok
    mutable_binding += 1;

    println!("After mutation: {}", mutable_binding); // 変更した後: {}

    // エラー!
    _immutable_binding += 1;
    // FIXME ^ この行をコメントアウトしてください。
}
```

コンパイラは可変性に関するエラーに対して詳細な診断をします。
