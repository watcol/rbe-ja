# `read_lines`

`lines()`メソッドはファイルを1行ずつ読み込むイテレータ
を返します。

`File::open`は`AsRef<Path>`を期待しますが、
`read_lines()`は入力を期待します。

```rust,no_run
use std::fs::File;
use std::io::{self, BufRead};
use std::path::Path;

fn main() {
    // ファイルが存在し、読み込めることが前提
    if let Ok(lines) = read_lines("./hosts") {
        // イテレータを消費し、文字列の`Result`を返す
        for line in lines {
            if let Ok(ip) = line {
                println!("{}", ip);
            }
        }
    }
}

// エラーに遭遇しても良いように、Resultを返す。
// BufReaderを行ごとに読み取るイテレータを返す。
fn read_lines<P>(filename: P) -> io::Result<io::Lines<io::BufReader<File>>>
where P: AsRef<Path>, {
    let file = File::open(filename)?;
    Ok(io::BufReader::new(file).lines())
}
```

このプログラムを実行すると、シンプルにすべての行の値が得られる。
```shell
$ echo -e "127.0.0.1\n192.168.0.1\n" > hosts
$ rustc read_lines.rs && ./read_lines
127.0.0.1
192.168.0.1
```

この方法は、特に大きなファイルを扱う時、`String`をすべてメモリに
保存するより効率的です。
