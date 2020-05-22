# `Result`の`map`

前の例の`multiply`でのパニックは実用コードのために作られていません。
普通、呼び出し元にエラーを返し、エラー処理を呼び出し元に任せます。

まず、どのようなタイプのエラーがあるのか知る必要があります。`Err`の型を決めるため、
[`FromStr`][from_str]トレイトを[`i32`][i32]に実装することで提供されている[`parse()`
][parse]関数を見てみます。すると、`Err`の型は[`ParseIntError`][parse_int_error]だと
定義されていることがわかります。

以下の例では、愚直に`match`文を使っていますが、これは面倒です。

```rust,editable
use std::num::ParseIntError;

// 返り値の型を書き直し、`unwrap()`を使わずパターンマッチしています。
fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    match first_number_str.parse::<i32>() {
        Ok(first_number)  => {
            match second_number_str.parse::<i32>() {
                Ok(second_number)  => {
                    Ok(first_number * second_number)
                },
                Err(e) => Err(e),
            }
        },
        Err(e) => Err(e),
    }
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    // これはまだ使えます。
    let twenty = multiply("10", "2");
    print(twenty);

    // これは有用なエラーメッセージを返すようになりました。
    let tt = multiply("t", "2");
    print(tt);
}
```

幸運なことに、`Option`の`map`や`and_then`などのコンビネータは`Result`にも
実装されています。[`Result`][result]に完全な一覧があります。

```rust,editable
use std::num::ParseIntError;

// `Option`のように、`map()`コンビネータが使えます。
// この関数は上と同じ動作をします。
// 値が有効な場合nを渡し、その他の場合はエラーを渡す。
fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    // これはまだ使えます。
    let twenty = multiply("10", "2");
    print(twenty);

    // これは有用なエラーメッセージを返すようになりました。
    let tt = multiply("t", "2");
    print(tt);
}
```

[parse]: https://doc.rust-lang.org/std/primitive.str.html#method.parse
[from_str]: https://doc.rust-lang.org/std/str/trait.FromStr.html
[i32]: https://doc.rust-lang.org/std/primitive.i32.html
[parse_int_error]: https://doc.rust-lang.org/std/num/struct.ParseIntError.html
[result]: https://doc.rust-lang.org/std/result/enum.Result.html
