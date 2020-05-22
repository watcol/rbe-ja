# `Result`を`Option`から引き出す

この問題の最も簡単な解決策は、どちらかがもう片方を埋め込むことです。

```rust,editable
use std::num::ParseIntError;

fn double_first(vec: Vec<&str>) -> Option<Result<i32, ParseIntError>> {
    vec.first().map(|first| {
        first.parse::<i32>().map(|n| 2 * n)
    })
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    println!("The first doubled is {:?}", double_first(numbers));

    println!("The first doubled is {:?}", double_first(empty));
    // エラー1: 入力ベクターが空です

    println!("The first doubled is {:?}", double_first(strings));
    // エラー2: 入力を数値に変換できません
}
```

エラーが起きた時に途中で処理を止めたい場合([`?`][enter_question_mark]などを使って)
`Option`はそのまま`None`を返します。 コンビネータが`Result`と`Option`を入れ替えるのに
役立ちます。

```rust,editable
use std::num::ParseIntError;

fn double_first(vec: Vec<&str>) -> Result<Option<i32>, ParseIntError> {
    let opt = vec.first().map(|first| {
        first.parse::<i32>().map(|n| 2 * n)
    });

    opt.map_or(Ok(None), |r| r.map(Some))
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    println!("The first doubled is {:?}", double_first(numbers));
    println!("The first doubled is {:?}", double_first(empty));
    println!("The first doubled is {:?}", double_first(strings));
}
```

[enter_question_mark]: ../result/enter_question_mark.md
