# ポインタ/参照

Rustのポインタは、`C`のような言語とは違い、間接参照と分割代入を区別する
必要があります。

 * 間接参照には`*`を使う
 * 分割代入には`&`、`ref`、`ref mut`を使う

```rust,editable
fn main() {
    // `i32`の参照を代入する。`&`で参照であることを
    // 明示している。
    let reference = &4;

    match reference {
        // `reference`が`&val`にマッチした時、次の2つが
        // 比較されている。
        // `&i32`
        // `&val`
        // ^ よって`&`が外れると、`i32`が
        // `val`に代入される。
        &val => println!("Got a value via destructuring: {:?}", val),  // 分割代入で値を取得しました: {:?}
    }

    // `&`をなくしたいときは、マッチする前に間接参照する
    match *reference {
        val => println!("Got a value via dereferencing: {:?}", val),  // 間接参照で値を取得しました: {:?}
    }

    // 参照を代入しない場合はどうでしょうか? `reference`は右辺値が`&`で
    // 始まっていたので参照でしたが、これは参照ではありません。
    let _not_a_reference = 3;

    // Rustはちょうどこの場合のために`ref`を提供しています。要素の参照が
    // 作られ、それが代入されます。
    let ref _is_a_reference = 3;

    // 同様に、参照を使わずに値を定義し、`ref`や`ref mut`で参照
    // を取得することができます。
    let value = 5;
    let mut mut_value = 6;

    // `ref`キーワードで参照を作りましょう
    match value {
        ref r => println!("Got a reference to a value: {:?}", r),  // 値の参照を取得しました
    }

    // 同様に`ref mut`を使います。
    match mut_value {
        ref mut m => {
            // 参照を取得して、値を変更するには間接参照する必要がある。
            *m += 10;
            println!("We added 10. `mut_value`: {:?}", m);  // 10加えました。`mut_value`: {:?}
        },
    }
}
```
