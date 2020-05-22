# イテレーションで`Result`を表す

`Iter::map`は失敗する可能性があります。例えば:

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Vec<_> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .collect();
    println!("Results: {:?}", numbers);
}
```

これを処理する方法を見ていきましょう。

## `filter_map()`で失敗したものを無視する

`filter_map`は`None`である結果をフィルタにかける。

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Vec<_> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .filter_map(Result::ok)
        .collect();
    println!("Results: {:?}", numbers);
}
```

## `collect()`でコードを失敗にする

`Result`は`FromIter`を実装しているため、`Result`型のベクター(`Vec<Result<T, E>>`)
をベクター型の`Result`(`Result<Vec<T>, E>`)に変換することができます。
そして、`Result::Err`が見つかれば処理を終了できます。

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Result<Vec<_>, _> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .collect();
    println!("Results: {:?}", numbers);
}
```

`Option`でも同じことができます。

## すべての利用可能な値を集めて`partition()`で失敗を振り分ける。

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let (numbers, errors): (Vec<_>, Vec<_>) = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .partition(Result::is_ok);
    println!("Numbers: {:?}", numbers);
    println!("Errors: {:?}", errors);
}
```

結果を見たら、これがまだ`Result`で包まれていることに気付くでしょう。
もう少し典型コードが必要です。

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let (numbers, errors): (Vec<_>, Vec<_>) = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .partition(Result::is_ok);
    let numbers: Vec<_> = numbers.into_iter().map(Result::unwrap).collect();
    let errors: Vec<_> = errors.into_iter().map(Result::unwrap_err).collect();
    println!("Numbers: {:?}", numbers);
    println!("Errors: {:?}", errors);
}
```
