# 早期のリターン

前の例で、コンビネータを使って明示的にエラーを処理していました。
もう一つの方法は、`match`文と*早期のリターン*を組み合わせて使うことです。

つまり、単純に一つエラーが起こればそこでエラーを返すということです。
しばしば、この方が読みやすく、書きやすいコードになります。早期のリターン
を使って前の例を書き直しました。

```rust,editable
use std::num::ParseIntError;

fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    let first_number = match first_number_str.parse::<i32>() {
        Ok(first_number)  => first_number,
        Err(e) => return Err(e),
    };

    let second_number = match second_number_str.parse::<i32>() {
        Ok(second_number)  => second_number,
        Err(e) => return Err(e),
    };

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

ここまでで、コンビネータや早期のリターンを使って、明示的にエラーを処理する方法を
見てきました。しかし、パニックを避け、明示的にすべてのエラーを処理するのは面倒です。

次の節では、`unwrap`のようにシンプルに、しかし`panic`をせずにエラーを処理する`?`
を導入します。
