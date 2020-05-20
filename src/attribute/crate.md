# クレート

`crate_type`属性でクレートがバイナリかライブラリか(そしてどのタイプのライブラリか)
を指定し、`crate_name`属性でクレート名を設定します。

しかし、`crate_type`や`crate_name`は、Cargoを使う場合は使わ**ない**方が良いことに
注意してください。これとCargoがメジャーなツールであることから、実用でこれが使われる
ケースは限られています。

```rust,editable
// このクレートはライブラリです
#![crate_type = "lib"]
// そしてライブラリ名は"rary"です
#![crate_name = "rary"]

pub fn public_function() {
    println!("called rary's `public_function()`");
}

fn private_function() {
    println!("called rary's `private_function()`");
}

pub fn indirect_access() {
    print!("called rary's `indirect_access()`, that\n> ");

    private_function();
}
```

`crate_type`属性を使えば、`--crate-type`フラグを`rustc`に
渡す必要はもはやありません。

```shell
$ rustc lib.rs
$ ls lib*
library.rlib
```
