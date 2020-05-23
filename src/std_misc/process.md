# 子プロセス

`process::Output`構造体は終了した子プロセスの出力を表します。
`process::Command`構造体はプロセスを作成します。

```rust,editable,ignore
use std::process::Command;

fn main() {
    let output = Command::new("rustc")
        .arg("--version")
        .output().unwrap_or_else(|e| {
            panic!("failed to execute process: {}", e)
    });

    if output.status.success() {
        let s = String::from_utf8_lossy(&output.stdout);

        print!("rustc succeeded and stdout was:\n{}", s);
    } else {
        let s = String::from_utf8_lossy(&output.stderr);

        print!("rustc failed and stderr was:\n{}", s);
    }
}
```

(間違ったフラグを`rustc`に渡すとエラーします。)
