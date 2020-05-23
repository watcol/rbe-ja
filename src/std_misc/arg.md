# プログラム引数

## 標準ライブラリ

標準ライブラリは`String`の各引数に対するイテレータを返す`std::env::args`
によってアクセスできます。

```rust,editable
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();

    // 最初の引数はプログラムの呼び出しに使われるものです。
    println!("My path is {}.", args[0]);

    // その他の引数はプログラムに与えられるオプションです。
    // このようにプログラムを呼び出します。
    //   $ ./args arg1 arg2
    println!("I got {:?} arguments: {:?}.", args.len() - 1, &args[1..]);
}
```

```shell
$ ./args 1 2 3
My path is ./args.
I got 3 arguments: ["1", "2", "3"].
```

## クレート

その他にも、コマンドラインアプリケーションを作成するのに役立つ多くのクレートがあります。
[Rust Cookbook]には、最もポピュラーなコマンドライン解析ライブラリ`clap`の使い方が書かれています。

[Rust Cookbook]: https://rust-lang-nursery.github.io/rust-cookbook/cli/arguments.html
