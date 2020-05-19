# ライブラリ

ライブラリを作り、他のクレートにリンクしてみましょう。

```rust,ignore
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

```shell
$ rustc --crate-type=lib rary.rs
$ ls lib*
library.rlib
```

ライブラリは"lib"で始まる必要があります。デフォルトでクレートファイル
の名前にちなんで名付けられます。しかし、これは`ructc`の`--crate-name`
オプションや、[`crate_name`属性][crate-name]で上書きできます。

[crate-name]: ../attribute/crate.md
