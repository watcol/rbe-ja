# クロージャ

Rustのクロージャ(ラムダ式やラムダとも呼ばれる)は環境をキャプチャできる関数です。
例えば、このクロージャはxをキャプチャします。
```Rust
|val| val + x
```

クロージャの構文と機能はその場での使用に便利です。関数を呼ぶようにクロージャ
を呼び出すことができます。しかし、入力値と返り値の型は推論できますが、入力の変数名は
指定する必要があります。

クロージャの他の特徴としては
* 入力変数名を`()`ではなく`||`で囲む。
* 式が一つのときは`{}`で囲む必要はありません(他は必須です)。
* 外部の変数をキャプチャできます。.

```rust,editable
fn main() {
    // 関数とクロージャでのインクリメント
    fn  function            (i: i32) -> i32 { i + 1 }

    // クロージャは名無しなので、参照に束縛します。
    // 注釈は関数の注釈と同じですが、本体を`{}`で囲む必要は
    // ありません。名無しのクロージャはは呼び出すために
    // 変数に代入する必要があります。
    let closure_annotated = |i: i32| -> i32 { i + 1 };
    let closure_inferred  = |i     |          i + 1  ;

    let i = 1;
    // 関数とクロージャを呼び出す。
    println!("function: {}", function(i));
    println!("closure_annotated: {}", closure_annotated(i));
    println!("closure_inferred: {}", closure_inferred(i));

    // 引数をとらず、`i32`を返すクロージャ。
    // 返り値は推論されます。
    let one = || 1;
    println!("closure returning one: {}", one());

}
```
