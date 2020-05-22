# `Result`

[`Result`][result]は値の*不在*を*エラー*に置き換えた[`Option`][option]型
の上位互換です。

つまり、`Result<T, E>`は2つの結果を持つことができます。

* `Ok(T)`: `T`が存在する場合
* `Err(E)`: `E`というエラーが見つかった場合

利便性のため、期待する値を`Ok`、予期せぬ値を`Err`に入れます。

`Option`と同じように、`Result`は多くのメソッドを持っています。例えば、
`unwrap()`は`T`型の値を返すか、`panic`します。また、条件処理のために、
`Option`と同じようなコンビネータが`Result`にもあります。

Rustを使っていると、おそらく、[`parse()`][parse]のように`Result`型を返す
ようなメソッドに出会うでしょう。文字列をいつも他の型に変換できるとは限らない
ため、`parse()`は失敗を処理するために`Result`を使っています。

成功した場合と失敗した場合の文字列の`parse()`の振る舞いを見てみましょう。

```rust,editable,ignore,mdbook-runnable
fn multiply(first_number_str: &str, second_number_str: &str) -> i32 {
    // `unwrap()`を使って数値を取り出してみましょう。
    let first_number = first_number_str.parse::<i32>().unwrap();
    let second_number = second_number_str.parse::<i32>().unwrap();
    first_number * second_number
}

fn main() {
    let twenty = multiply("10", "2");
    println!("double is {}", twenty);

    let tt = multiply("t", "2");
    println!("double is {}", tt);
}
```

`parse()`が失敗した場合は、`unwrap()`で`panic`を起こしてプログラムを終了しました。
さらに、`panic`は不快なエラーメッセージを返しました。

エラーメッセージの質を改善するため、返り値の型をさらに特定し、明示的に
エラーを処理する必要があります。

## `main`で`Result`を使う

明示的に指定することで、`Result`型を`main`関数で使うこともできます。
典型的な`main`関数はこのような形です。

```rust
fn main() {
    println!("Hello World!");
}
```

しかし、`main`は返り値として`Result`を使うこともできます。`main`関数内でエラーが
起これば、エラーコードを返し、デバッグプリントでエラーを報告します。([`Debug`]
トレイトを使います。) 以下の例では、次の例では、そのようなシナリオを紹介し、
[後の節][the following section]で扱う内容にも触れます。

```rust,editable
use std::num::ParseIntError;

fn main() -> Result<(), ParseIntError> {
    let number_str = "10";
    let number = match number_str.parse::<i32>() {
        Ok(number)  => number,
        Err(e) => return Err(e),
    };
    println!("{}", number);
    Ok(())
}
```


[option]: https://doc.rust-lang.org/std/option/enum.Option.html
[result]: https://doc.rust-lang.org/std/result/enum.Result.html
[parse]: https://doc.rust-lang.org/std/primitive.str.html#method.parse
[`Debug`]: https://doc.rust-lang.org/std/fmt/trait.Debug.html
[the following section]: result/early_returns.md
