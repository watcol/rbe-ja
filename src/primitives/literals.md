# リテラルと演算子

整数の`1`、浮動小数点数の`1.2`、文字の`'a'`、文字列の`"abc"`、真偽値の`true`、
そしてユニット型の`()`などは、リテラルで表すことができます。

整数は`0x`、`0o`、`0b`などのプレフィックスをつけることで、
16進数、8進数、2進数でも表すことができます。

また、可読性のため、数値にアンダースコアを挟むことができます。 例えば
`1_000`は`1000`と同じで、`0.000_001`は`0.000001`と同じです。

コンパイラにリテラルの型を教えてあげなければいけません. 現在は、
`u32`サフィックスでリテラル32ビット符号なし整数になり、`i32`サフィックス
で32ビット符号付き整数になります。

Rustで使える[演算子や、その優先順位][rust op-prec]は、[Cのような言語][op-prec]
と似ています。

```rust,editable
fn main() {
    // 整数の足し算
    println!("1 + 2 = {}", 1u32 + 2);

    // 整数の引き算
    println!("1 - 2 = {}", 1i32 - 2);
    // TODO ^ `1i32`を`1u32`に変えてみて、型の重要さを実感してみてください。

    // 真偽値の短絡評価
    println!("true AND false is {}", true && false);
    println!("true OR false is {}", true || false);
    println!("NOT true is {}", !true); // 真でないものは{}

    // ビット演算
    println!("0011 AND 0101 is {:04b}", 0b0011u32 & 0b0101);
    println!("0011 OR 0101 is {:04b}", 0b0011u32 | 0b0101);
    println!("0011 XOR 0101 is {:04b}", 0b0011u32 ^ 0b0101);
    println!("1 << 5 is {}", 1u32 << 5);
    println!("0x80 >> 2 is 0x{:x}", 0x80u32 >> 2);

    // アンダースコアを使って可読性を上げましょう!
    println!("One million is written as {}", 1_000_000u32); // 100万は{}と書きます
}
```

[rust op-prec]: https://doc.rust-lang.org/reference/expressions.html#expression-precedence
[op-prec]: https://en.wikipedia.org/wiki/Operator_precedence#Programming_languages
