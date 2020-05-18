# while let

`if let`と同様に、`while let`も冗長な`match`文を簡略化するのに使えます。
`i`をインクリメントする次の節を見てください。

```rust
// `Option<i32>`型の`optional`を作ります
let mut optional = Some(0);

// この試行を繰り返します。
loop {
    match optional {
        // `optional`がSome(i)にマッチすれば、このブロックを実行します。
        Some(i) => {
            if i > 9 {
                println!("Greater than 9, quit!");  // 0より大きいので、終了します!
                optional = None;
            } else {
                println!("`i` is `{:?}`. Try again.", i);  // `i`は`{:?}`です。もう1度試してください。
                optional = Some(i + 1);
            }
            // ^ 3インデント必要です!
        },
        // マッチしなければ、ループを出ます。
        _ => { break; }
        // ^ これは必要でしょうか? もっと良い方法があります!
    }
}
```

`while let`を使えば、この節をより良くなります。

```rust,editable
fn main() {
    // `Option<i32>`型の`optional`を作ります
    let mut optional = Some(0);

    // 「`let`が`optional`を`Some(i)`に分解できる限り、ブロック(`{}`)を実行し、
    // そうでなければ`break`する」という意味
    while let Some(i) = optional {
        if i > 9 {
            println!("Greater than 9, quit!");
            optional = None;
        } else {
            println!("`i` is `{:?}`. Try again.", i);
            optional = Some(i + 1);
        }
        // ^ インデントも少なく、明示的な失敗の処理も不要です。
    }
    // ^ `if let`は`else`/`else if`節を持っていますが、
    // `while let`にはありません。
}
```

### こちらも参照:

- [`enum`][enum]
- [`Option`][option]
- [RFC][while_let_rfc]

[enum]: ../custom_types/enum.md
[option]: ../std/option.md
[while_let_rfc]: https://github.com/rust-lang/rfcs/pull/214
