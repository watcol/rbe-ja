# `panic`

最もシンプルなエラー処理は`panic`です。これはエラーメッセージを表示し、
スタックを巻き戻し、通常はプログラムを終了します。
ここでは、エラーしたときに`panic`を直接呼び出しています。

```rust,editable,ignore,mdbook-runnable
fn give_princess(gift: &str) {
    // プリンセスはヘビが嫌いです。なので、彼女が拒否したときに
    // 止めてあげる必要があります!
    if gift == "snake" { panic!("AAAaaaaa!!!!"); }

    println!("I love {}s!!!!!", gift);
}

fn main() {
    give_princess("teddy bear");
    give_princess("snake");
}
```
