# Rust by Example

[Rust][rust]は安全性、速度、並行性に焦点を当てたモダンなシステム
プログラミング言語です。これができるのは、ガベージコレクションなしでメモリ
安全であるためです。

Rust by Example (RBE)は、Rustの様々な概念や標準ライブラリを実行できる例で紹介
したサンプルコード集です。この例を活用するために、[Rustをローカルに
インストール][install]して、[公式ドキュメント][std]を参照することを忘れないで
ください。
興味がある方は[このサイトのソースコード][home]もあります。(日本語版は[こちら
][home-ja]。)

それでは、始めましょう!

- [Hello World](hello.md) - 伝統的な「Hello World」プログラムから始めます。

- [プリミティブ](primitives.md) - 符号付き整数、符号なし整数などのプリミティブについて学びます。

- [カスタム型](custom_types.md) - `struct`と`enum`

- [変数束縛](variable_bindings.md) - 可変束縛、スコープ、シャドーイング。

- [型](types.md) - 型の変更と宣言について学びます。

- [変換](conversion.md)

- [式](expression.md)

- [制御フロー](flow_control.md) - `if`/`else`、`for`など。

- [関数](fn.md) - メソッド、クロージャ、高階関数などについて学びます。

- [モジュール](mod.md) - モジュールでコードを整理する。

- [クレート](crates.md) - クレートはRustの編集ユニットです。ライブラリの作り方を学びます。

- [Cargo](cargo.md) - Rustの標準パッケージマネージャの基本的な機能について学びます。

- [属性](attribute.md) - 属性とは、モジュールやクレート、その要素などに対して適用されるメタデータのことです。

- [ジェネリック](generics.md) - 複数の型の引数に対して実行できる関数やデータ型について学びます.

- [スコープのルール](scope.md) - スコープは所有権、借用、ライフタイムに関して重要な役割を担います。

- [トレイト](trait.md) - トレイとは未知の型`Self`に対して実装されたメソッドの集合です。

- [マクロ](macros.md)

- [エラー処理](error.md) - Rustで失敗を処理する方法について学びます。

- [Stdライブラリの型](std.md) - `std`ライブラリで提供されるいくつかのカスタム型について学びます。

- [その他のStd](std_misc.md) - ファイル処理、スレッドなどのその他のカスタム型。

- [テスト](testing.md) - Rustにおけるすべての種類のテスト。

- [安全でない操作](unsafe.md)

- [互換性](compatibility.md)

- [メタデータ](meta.md) - ドキュメント、ベンチマーク。


[rust]: https://www.rust-lang.org/
[install]: https://www.rust-lang.org/tools/install
[std]: https://doc.rust-lang.org/std/
[home]: https://github.com/rust-lang/rust-by-example
[home-ja]: https://github.com/watcol/rbe-ja
