# 変数束縛

Rustは静的型付けによる型安全性を提供しています。宣言時に型注釈をつけることもできますが、
ほとんどの場合、注釈の労力を軽減するため、コンパイラが変数の型を文脈から推論することが
できます。

(リテラルのような)値は、`let`を使って変数に束縛することができます。

```rust,editable
fn main() {
    let an_integer = 1u32;
    let a_boolean = true;
    let unit = ();

    // `an_integer`を`copied_integer`にコピーする
    let copied_integer = an_integer;

    println!("An integer: {:?}", copied_integer);
    println!("A boolean: {:?}", a_boolean);
    println!("Meet the unit value: {:?}", unit);

    // コンパイラは、使われていない変数の束縛に対して警告をします。
    // 変数名の最初にアンダースコアを付けることによって無効化できます。
    let _unused_variable = 3u32;

    let noisy_unused_variable = 2u32;
    // FIXME ^ 警告を消すため、アンダースコアを付けてください。
}
```
