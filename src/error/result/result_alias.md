# `Result`のエイリアス

特定の`Result`型を何回も使い回すときはどうでしょう?
前に言ったようにRustでは[型エイリアス][typealias]が使えるため、
特定の`Result`型を再定義することができます。

モジュールレベルで型エイリアスを作ると非常に便利です。特定のモジュールでは
同じ`Err`型を使うことが多いからです。これによって*すべて*の関連した`Result`
に一つの型エイリアスを使うことができます。`std`ライブラリもこのような型[`io::Result`
][io_result]を定義しています!

これは構文を紹介するための簡単な例です。

```rust,editable
use std::num::ParseIntError;

// `ParseIntError`を持つ`Result`型のジェネリックなエイリアス
type AliasedResult<T> = Result<T, ParseIntError>;

// 特定のエラー型を持つ`Result`型を使ってみましょう。
fn multiply(first_number_str: &str, second_number_str: &str) -> AliasedResult<i32> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

// ここでコード量を節約できます。
fn print(result: AliasedResult<i32>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    print(multiply("10", "2"));
    print(multiply("t", "2"));
}
```

### こちらも参照:

- [`io::Result`][io_result]

[typealias]: ../../types/alias.md
[io_result]: https://doc.rust-lang.org/std/io/type.Result.html
