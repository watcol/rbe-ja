# match

RustはCの`switch`のような、`match`キーワードによる条件分岐を提供しています。

```rust,editable
fn main() {
    let number = 13;
    // TODO ^ `number`を違う値にして試してみてください。

    println!("Tell me about {}", number);  // {}について教えて
    match number {
        //一つの値にマッチする
        1 => println!("One!"),
        // いくつかの値にマッチする
        2 | 3 | 5 | 7 | 11 => println!("This is a prime"),
        // 最後の値を含むrangeにマッチする
        13..=19 => println!("A teen"),
        // Handle the rest of cases
        _ => println!("Ain't special"),
    }

    let boolean = true;
    // Matchも式です
    let binary = match boolean {
        // matchのアームはとりうるすべての値を網羅しなければなりません。
        false => 0,
        true => 1,
        // TODO ^ 上のどちらかをコメントアウトしてしてください。
    };

    println!("{} -> {}", boolean, binary);
}
```
