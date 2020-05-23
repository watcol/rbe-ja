# `open`

`open`静的メソッドはファイルを読み取り専用モードで開くのに使うことができます。

`File`はリソース、ファイル記述子を所有していて、いつファイルが`drop`されるかに
ついての配慮をしています。

```rust,editable,ignore
use std::fs::File;
use std::io::prelude::*;
use std::path::Path;

fn main() {
    // 開くファイルのパスを作る
    let path = Path::new("hello.txt");
    let display = path.display();

    // 読み取り専用でパスを開き、`io::Result<File>`を返す。
    let mut file = match File::open(&path) {
        Err(why) => panic!("couldn't open {}: {}", display, why),
        Ok(file) => file,
    };

    // ファイルを文字列に読み込み、`io::Result<usize>`を返す。
    let mut s = String::new();
    match file.read_to_string(&mut s) {
        Err(why) => panic!("couldn't read {}: {}", display, why),
        Ok(_) => print!("{} contains:\n{}", display, s),
    }

    // `file`がスコープを出たことで、"hello.txt"は閉じられます。
}
```

これが成功時の出力です。

```shell
$ echo "Hello World!" > hello.txt
$ rustc open.rs && ./open
hello.txt contains:
Hello World!
```

(`hello.txt`が存在しない、または`hello.txt`が読み込めないなどの
場合、このプログラムはエラーします。)
