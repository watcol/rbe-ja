# Hello World

こちらが伝統的な「Hello World」プログラムのソースコードです。

```rust,editable
// これはコメントで、コンパイラに無視されます。
// ここにある"Run"ボタンをクリックするとコードをテストできます ->
// キーボードを使いたければ、"Ctrl + Enter"ショートカットを使うことができます

// このコードは編集できます。自由にハックしてください!
// "Reset"ボタンをクリックするといつでも元のコードに戻すことができます ->

// これがmain関数です
fn main() {
    // ここにある式はバイナリが呼び出されたときに実行されます。

    // テキストをコンソールに出力します
    println!("Hello World!");
}
```

`println!`はテキストをコンソールに出力する[*マクロ*][macros]です。

Rustのコンパイラ`rustc`を使うことでバイナリを生成できます。

```bash
$ rustc hello.rs
```

`rustc`は`hello`実行可能バイナリを出力します。

```bash
$ ./hello
Hello World!
```

### 演習

'Run'で出力を確認してください. 次に, このように出力する
、2つ目の`println!`マクロを使った行を追加してください。

```text
Hello World!
I'm a Rustacean!
```

[macros]: macros.md
