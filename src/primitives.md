# プリミティブ

Rustは、様々な`プリミティブ`を提供しています。以下がその例です。


### スカラー型

* 符号付き整数: `i8`、`i16`、`i32`、`i64`、`i128`、そして`isize` (ポインタサイズ)
* 符号なし整数: `u8`、`u16`、`u32`、`u64`、`u128`、そして`usize` (ポインタサイズ)
* 浮動小数点: `f32`, `f64`
* `char` Unicodeのスカラー値。例えば`'a'`、`'α'`、そして`'∞'` (それぞれ4バイト)
* `bool` `true`または`false`
* ユニット型。唯一の値が空タプル`()`。

ユニット型はタプルですが、複数の値を含むことができないため、
複合型ではありません。

### 複合型

* `[1, 2, 3]`のような配列
* `(1, true)`のようなタプル

変数は常に*型指定*可能です。数値型は更に*サフィックス*の指定が可能で、
指定しない場合はデフォルトになります。整数値のデフォルトは`i32`で、
浮動小数点数は`f64`です。Rustはさらに文脈から型推論ができることに注意
してください。

```rust,editable,ignore,mdbook-runnable
fn main() {
    // 変数には型の注釈が付けられます。
    let logical: bool = true;

    let a_float: f64 = 1.0;  // ふつうの注釈
    let an_integer   = 5i32; // サフィックスで注釈

    // デフォルトを選択することもできます。
    let default_float   = 3.0; // `f64`
    let default_integer = 7;   // `i32`
    
    //  文脈から型推論する
    let mut inferred_type = 12; // 他の行からi64であると推論する。
    inferred_type = 4294967296i64;
    
    // 可変な変数は変更できます。
    let mut mutable = 12; // 可変な`i32`
    mutable = 21;
    
    // エラーします! 変数の型は変更できません。
    mutable = true;
    
    // 変数はシャドーイングで上書きできます。
    let mutable = true;
}
```

### こちらも参照:

- [`std`ライブラリ][std]
- [`mut`][mut]
- [推論][inference]
- [シャドーイング][shadowing]

[std]: https://doc.rust-lang.org/std/
[mut]: variable_bindings/mut.md
[inference]: types/inference.md
[shadowing]: variable_bindings/scope.md
