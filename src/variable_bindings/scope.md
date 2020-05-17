# スコープとシャドーイング

変数束縛は、スコープを持ち、それは*ブロック*内で有効です。ブロックは、
波括弧`{}`でくくられた文の集合のことです。さらに、[変数のシャドーイング
][variable-shadow]も許可されています。

```rust,editable,ignore,mdbook-runnable
fn main() {
    // この束縛はmain関数内で有効です。
    let long_lived_binding = 1;

    // これはブロックで、main関数より小さいスコープを持っています。
    {
        // このブロック内でのみ有効。
        let short_lived_binding = 2;

        println!("inner short: {}", short_lived_binding);

        // 外側の束縛を*覆い隠す*ことができます。
        let long_lived_binding = 5_f32;

        println!("inner long: {}", long_lived_binding);
    }
    // ブロックの終わり

    // エラー! `short_lived_binding`はこのスコープに存在しません。
    println!("outer short: {}", short_lived_binding);
    // FIXME ^ この行をコメントアウトしてください。

    println!("outer long: {}", long_lived_binding);
    
    // この束縛も前の束縛を*覆い隠し*ます。
    let long_lived_binding = 'a';
    
    println!("outer long: {}", long_lived_binding);
}
```

[variable-shadow]: https://en.wikipedia.org/wiki/Variable_shadowing
