# フォーマットしてプリント

プリントは[`std::fmt`][fmt]で定義されているいくつかの[`マクロ`][macros]によって
制御されています。そのいくつかを紹介します。

* `format!`: フォーマットされたテキストを[`String`][string]に書き込む。
* `print!`: `format!`と同様だが、コンソール(io::stdout)に書き込む。
* `println!`: `print!`と同様だが、改行が追加される。
* `eprint!`: `format!`と同様だが、標準エラー出力(io::stderr)に書き込む。
* `eprintln!`: `eprint!`と同様だが、改行が追加される。

すべてのものは、同じやり方で解析し、 コンパイル時に正しくフォーマットできるか
チェックします。

```rust,editable,ignore,mdbook-runnable
fn main() {
    // 一般的に、`{}`が自動的に引数に変換されます。
    // 自動的に文字列に変換します。
    println!("{} days", 31); // {}日

    // 示唆しなければ、31はi32になります。示唆を加えることで、31の型を
    // 変えられます。例えば31i64はi64として解釈されます。

    // 引数の位置を使って、埋め込まれる場所を指定することができます。
    println!("{0}, this is {1}. {1}, this is {0}", "Alice", "Bob"); // {0}、こちらが{1}です。{1}、こちらが{0}です。

    // 名前を付けても良いです。
    println!("{subject} {verb} {object}", // 素早い茶色の狐はのろまな犬を飛び越える
             object="the lazy dog",
             subject="the quick brown fox",
             verb="jumps over");

    // `:`の後にフォーマット型を指定すると特殊なフォーマットができます。
    println!("{} of {:b} people know binary, the other half doesn't", 1, 2); // {:b}(2つ目)人に{}(1つ目)人はバイナリを知っていますが、残りの半分は知りません。

    // 幅を指定して右寄せにできます。 この例は
    // "     1". 5 white spaces and a "1"と出力します。
    println!("{number:>width$}", number=1, width=6);

    // 余分なゼロで数字を埋めることができます。この例は"000001"と出力します。
    println!("{number:>0width$}", number=1, width=6);

    // Rustは指定された数の引数が使われているかもチェック
    // します。
    println!("My name is {0}, {1} {0}", "Bond"); // 私の名前は{0}、{1} {0}です。
    // FIXME ^ 足りない引数、"James"を追加してください。

    // `i32`を含む`Structure`という構造体を作ります。
    #[allow(dead_code)]
    struct Structure(i32);

    // しかし、この構造体のようなカスタム型はもう少し複雑です。
    // これは動作しません。
    println!("This struct `{}` won't print...", Structure(3)); // この構造体`{}`は出力できません...
    // FIXME ^ この行をコメントアウトしてください。
}
```

[`std::fmt`][fmt]はテキストを出力するための多くの[`トレイト`][traits]を持って
います。2つの重要なものを挙げます。

* `fmt::Debug`: `{:?}`マーカーを使います。デバッグ用にテキストをフォーマットします。
* `fmt::Display`: `{}`マーカーを使います。もっと美しく、ユーザーフレンドリーに
表示します。

この例で使われている型は、標準ライブラリに含まれているため、ここでは`fmt::Display`
を使用しています。カスタム型を扱う場合はもう少し複雑です。

`fmt::Display`トレイトを実装すると、自動的に[`String`][string]に[変換][convert]する
[`ToString`]トレイトが実装されます。

### 演習

 * 上のコードの、2つの箇所を変更して(FIXMEを見てください)エラーなく実行できる
   ようにしてください。
 * 表示される小数の桁数を調整し、`Pi is roughly 3.142`(Piは大体3.142です)
   と出力される`println!`マクロを追加してください。ただし、円周率の値は
   `let pi = 3.141592`を使ってください。(ヒント: 小数の桁数を調整して出力
   する方法について、[`std::fmt`][fmt]ドキュメントをチェックしてみてください。)

### こちらも参照:

- [`std::fmt`][fmt]
- [`macros`][macros]
- [`structs`][structs]
- [`traits`][traits]

[fmt]: https://doc.rust-lang.org/std/fmt/
[macros]: ../macros.md
[string]: ../std/str.md
[structs]: ../custom_types/structs.md
[traits]: https://doc.rust-lang.org/std/fmt/#formatting-traits
[`ToString`]: https://doc.rust-lang.org/std/string/trait.ToString.html
[convert]: ../conversion/string.md
