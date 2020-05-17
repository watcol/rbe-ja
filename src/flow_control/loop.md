# loop

Rustは、無限ループを作る`loop`キーワードを提供しています。

`break`文でいつでもループを抜けることができ、`continue`文で残りの
部分を飛ばして次の繰り返しに進むことができます。

```rust,editable
fn main() {
    let mut count = 0u32;

    println!("Let's count until infinity!");  // 無限まで数えましょう!

    // 無限ループ
    loop {
        count += 1;

        if count == 3 {
            println!("three");

            // 残りの処理を飛ばして次に行く。
            continue;
        }

        println!("{}", count);

        if count == 5 {
            println!("OK, that's enough");  // OK, これで十分

            // このループを抜ける
            break;
        }
    }
}
```
