# 宣言

変数を最初に宣言して、後で初期化することができます。
しかし、初期化されていない変数を使う危険性があるので、めったに
使いません。

```rust,editable,ignore,mdbook-runnable
fn main() {
    // 変数束縛の宣言
    let a_binding;

    {
        let x = 2;

        // 束縛の初期化
        a_binding = x * x;
    }

    println!("a binding: {}", a_binding);

    let another_binding;

    // エラー! 初期化されていない変数を使用しています。
    println!("another binding: {}", another_binding);
    // FIXME ^ この行をコメントアウトする

    another_binding = 1;

    println!("another binding: {}", another_binding);
}
```

未定義な動作をする危険があるので、コンパイラは初期化されていない変数の使用を禁止しています。
