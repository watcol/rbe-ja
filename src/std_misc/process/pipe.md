# パイプ

`std::Child`構造体は実行中の子プロセスを表し、`stdin`、`stdout`や`stderr`
を制御してプロセスをパイプしてつなぐことができます。

```rust,ignore
use std::io::prelude::*;
use std::process::{Command, Stdio};

static PANGRAM: &'static str =
"the quick brown fox jumped over the lazy dog\n";

fn main() {
    // `wc`コマンドを実行する。
    let process = match Command::new("wc")
                                .stdin(Stdio::piped())
                                .stdout(Stdio::piped())
                                .spawn() {
        Err(why) => panic!("couldn't spawn wc: {}", why),
        Ok(process) => process,
    };

    // `wc`の`stdin`を書き込む。
    //
    // `stdin`は`Option<ChildStdin>`型ですが、インスタンスが存在することが
    // 分かるため、`unwrap`できます。
    match process.stdin.unwrap().write_all(PANGRAM.as_bytes()) {
        Err(why) => panic!("couldn't write to wc stdin: {}", why),
        Ok(_) => println!("sent pangram to wc"),
    }

    // `stdin`は上の呼び出しの後、使われないため`drop`されます。
    // そしてパイプを閉じます。
    //
    // これをしないと、 入力を送っただけなので、`wc`は
    // 処理を開始します。

    // `stdout`も`Option<ChildStdout>`型なので、unwrapしないといけません。
    let mut s = String::new();
    match process.stdout.unwrap().read_to_string(&mut s) {
        Err(why) => panic!("couldn't read wc stdout: {}", why),
        Ok(_) => print!("wc responded with:\n{}", s),
    }
}
```
