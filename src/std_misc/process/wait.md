# ウェイト

`process::Child`は子プロセスが終了するのを待つことができます。
`process::ExitStatus`を返す`Child::wait()`を呼び出してください。

```rust,ignore
use std::process::Command;

fn main() {
    let mut child = Command::new("sleep").arg("5").spawn().unwrap();
    let _result = child.wait().unwrap();

    println!("reached end of main");
}
```

```bash
$ rustc wait.rs && ./wait
# `wait`は`sleep 5`コマンドが終了するまで5秒待ちます。
reached end of main
```
