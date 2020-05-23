# `Option`

時々`panic!`を使わずにエラーを処理したいことがあります。
これは`Option`enumを使うことで実現できます。

`Option<T>`は2つの列挙子を持ちます。

* `None`は値がないことをあらわし、
* `Some(value)`は`T`型の`value`を持つタプル構造体です。

```rust,editable,ignore,mdbook-runnable
// `panic!`しない整数の割り算
fn checked_division(dividend: i32, divisor: i32) -> Option<i32> {
    if divisor == 0 {
        // 失敗したときは`None`列挙子を返す
        None
    } else {
        // 結果は`Some`列挙子に入れて返します
        Some(dividend / divisor)
    }
}

// この関数は割り算が成功したか確認します。
fn try_division(dividend: i32, divisor: i32) {
    // `Option`の値は他のenumと同じようにパターンマッチできる。
    match checked_division(dividend, divisor) {
        None => println!("{} / {} failed!", dividend, divisor),
        Some(quotient) => {
            println!("{} / {} = {}", dividend, divisor, quotient)
        },
    }
}

fn main() {
    try_division(4, 2);
    try_division(1, 0);

    // `None`を変数に代入する時は型注釈が必要です
    let none: Option<i32> = None;
    let _equivalent_none = None::<i32>;

    let optional_float = Some(0f32);

    // `Some`列挙子でラップされた値を取り出す。
    println!("{:?} unwraps to {:?}", optional_float, optional_float.unwrap());

    // `None`列挙子だった場合は`panic!`する
    println!("{:?} unwraps to {:?}", none, none.unwrap());
}
```
