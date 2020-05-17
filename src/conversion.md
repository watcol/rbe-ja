# 変換

プリミティブ型は[キャスト][casting]で変換できます。

Rustはカスタム型(`struct`と`enum`のこと)の変換を[トレイト][traits]を使って
処理しています。汎用的な変換は[`From`]トレイトや[`Into`]トレイトを使って行われます。
しかし、 特に`String`へ、`String`からの変換など、より一般的なケースのため、より
具体的な変換が必要になることがあります。

[casting]: types/cast.md
[traits]: trait.md
[`From`]: https://doc.rust-lang.org/std/convert/trait.From.html
[`Into`]: https://doc.rust-lang.org/std/convert/trait.Into.html
