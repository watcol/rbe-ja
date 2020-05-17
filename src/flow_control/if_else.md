# if/else

`if`-`else`での条件分岐は他の言語とほぼ同じです。多くの言語と違って、
条件を括弧で囲む必要はありません。また、条件の後にはブロックが続きます。
`if`-`else`は式の一種であり、すべての枝が同じ型を返す必要があります。

```rust,editable
fn main() {
    let n = 5;

    if n < 0 {
        print!("{} is negative", n);  // {}は負です
    } else if n > 0 {
        print!("{} is positive", n);  // {}は正です
    } else {
        print!("{} is zero", n);  // {}はゼロです
    }

    let big_n =
        if n < 10 && n > -10 {
            println!(", and is a small number, increase ten-fold");  // そして、小さいので、10倍します

            // この式は`i32`を返します。
            10 * n
        } else {
            println!(", and is a big number, halve the number");  // そして、大きいので、半分にします

            // この式は`i32`を返す必要があります
            n / 2
            // TODO ^ この式にセミコロンを付けてみてください。
        };
    //   ^ ここにセミコロンを付けるのを忘れないでください! すべての`let`文に必要です。

    println!("{} -> {}", n, big_n);
}
```
