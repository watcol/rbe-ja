# ネストとラベル

ネストされたループを扱っている時、外側のループを`break`、`continue` できます。
その時は、ループはラベル`'label`で注釈されている必要があり、`break`/`continue`文
にラベルを渡さなければいけません。

```rust,editable
#![allow(unreachable_code)]

fn main() {
    'outer: loop {
        println!("Entered the outer loop");  // 外側のループに入りました

        'inner: loop {
            println!("Entered the inner loop");  // 内側のループに入りました

            // 内側のループだけから抜けます
            //break;

            // 外側のループを抜けます
            break 'outer;
        }

        println!("This point will never be reached");  // ここは決して実行されません
    }

    println!("Exited the outer loop");  // 外側のループを出ました
}
```
