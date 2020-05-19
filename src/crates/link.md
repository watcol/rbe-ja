# `extern crate`

クレートにライブラリをリンクするには、`extern crate`宣言が必要です。
これはライブラリをリンクするだけでなく、ライブラリのモジュールを
ライブラリ名と同じ名前でインポートします。可視性のルールはモジュール
だけでなくライブラリにも適用されます。

```rust,ignore
// `library`とリンクし、`rary`モジュールの要素をインポートします。
extern crate rary;

fn main() {
    rary::public_function();

    // エラー! `private_function`はプライベートです
    //rary::private_function();

    rary::indirect_access();
}
```

```txt
# library.rlibのようなコンパイルされたライブラリのパスは、
# 同じディレクトリにあることが想定されています。
$ rustc executable.rs --extern rary=library.rlib && ./executable
called rary's `public_function()`
called rary's `indirect_access()`, that
> called rary's `private_function()`
```
