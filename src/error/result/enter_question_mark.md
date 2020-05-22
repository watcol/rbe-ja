# `?`の導入

時々、`panic`なしに、`unwrap`のような単純さが欲しくなることがあります。
今まで、変数を取得するためだけに、ますます深くネストすることが必要でした。
これをなくすのが`?`の目的です。

`Err`を見つけると、このような挙動を取ることができます。

1. `panic!`。これはできるだけ避けるべきです。
2. `return`。`Err`は処理できないことを意味するため、`return`する。

`?`は*ほとんど*[^†] `unwrap`の`panic`を`return`に変えたもとと等価です。
先程の例がどれだけシンプルになるか見てみましょう。

```rust,editable
use std::num::ParseIntError;

fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    let first_number = first_number_str.parse::<i32>()?;
    let second_number = second_number_str.parse::<i32>()?;

    Ok(first_number * second_number)
}

fn print(result: Result<i32, ParseIntError>) {
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

## `try!`マクロ

`?`がなかった頃、同じ機能を`try!`マクロが持っていました。現在は`?`演算子が推奨されていますが、
古いコードを見ていると、`try!`に遭遇するかもしれません。前の例と同じ`multiply`関数を`try!`で
実装してみます。

```rust,editable
// この例を警告なしに実行するにはCargo.tomlの`[package]`節にある`edition`フィールドを
// "2015"に変更する必要があります。

use std::num::ParseIntError;

fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    let first_number = try!(first_number_str.parse::<i32>());
    let second_number = try!(second_number_str.parse::<i32>());

    Ok(first_number * second_number)
}

fn print(result: Result<i32, ParseIntError>) {
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


[^†]: 詳しくは[他の?の使い方][re_enter_?]を参照してください。

[re_enter_?]: ../multiple_error_types/reenter_question_mark.md
