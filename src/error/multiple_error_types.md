# 複数のエラー型

`Result`が他の`Result`と、`Option`が他の`Option`と連携が取れるため、
前の例は非常に便利でした。

さて、時々`Option`と`Result`、`Result<T, Error1>`と`Result<T, Error2>`の間で
連携を取る必要があるときがあります。この場合、異なるエラー型を簡単で構成可能
に管理する必要があります。

以下のコードでは、`Vec::first`は`Option`を、`parse::<i32>`は`Result<i32, ParseIntError>`
を返すため、2つの`unwrap`がそれぞれ異なるエラーを処理しています。

```rust,editable,ignore,mdbook-runnable
fn double_first(vec: Vec<&str>) -> i32 {
    let first = vec.first().unwrap(); // Generate error 1
    2 * first.parse::<i32>().unwrap() // Generate error 2
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    println!("The first doubled is {}", double_first(numbers));

    println!("The first doubled is {}", double_first(empty));
    // エラー1: 入力ベクターが空です。

    println!("The first doubled is {}", double_first(strings));
    // エラー2: 入力を数値に変換できません。
}
```

次の節から、このような問題を処理するいくつかの方法を見ていきます。
