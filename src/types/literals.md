# リテラル

数値リテラルはサフィックスで型の注釈が付けられます。例として、
`42`というリテラルの型を`i32`であると指定するには、`42i32`と書きます。

サフィックスされていない数値リテラルの型は、それががどう使われるかによります。もし
制約がなければ、コンパイラは整数に`i32`を、浮動小数点数に`f64`を使います。

```rust,editable
fn main() {
    // サフィックスされたリテラル。これらの型は初期化時に特定されます。
    let x = 1u8;
    let y = 2u32;
    let z = 3f32;

    // サフィックスされていないリテラル。どのように使われるかによって型が決まる。
    let i = 1;
    let f = 1.0;

    // `size_of_val` は変数のバイト数を返します。
    println!("size of `x` in bytes: {}", std::mem::size_of_val(&x));
    println!("size of `y` in bytes: {}", std::mem::size_of_val(&y));
    println!("size of `z` in bytes: {}", std::mem::size_of_val(&z));
    println!("size of `i` in bytes: {}", std::mem::size_of_val(&i));
    println!("size of `f` in bytes: {}", std::mem::size_of_val(&f));
}
```

このコードにはまだ説明していないいくつかのまだ説明していない概念が使われています。
ここで短気な読者のために簡単に説明します。

* `std::mem::size_of_val`は関数ですが、*フルパス*で呼び出されています。コードは
  *モジュール*と呼ばれる論理ユニットで分割できます。ここでは、`size_of_val`関数は
  `mem`モジュールで定義され、`mem`モジュールは`std`*クレート*で定義されています。
  詳細は、[モジュール][mod]と[クレート][crate]を見てください。

[mod]: ../mod.md
[crate]: ../crates.md
