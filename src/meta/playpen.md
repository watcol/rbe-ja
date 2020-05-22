# Playpen

[Rust Playpen](https://github.com/rust-lang/rust-playpen)はRustコードをWeb上で実験するのに有用です。
このプロジェクトは[Rust Playground](https://play.rust-lang.org/)とも呼ばれています。

## `mdbook`から使用する

[`mdbook`][mdbook]上では、コード例を実行、編集可能にすることができます。

```rust,editable
fn main() {
    println!("Hello World!");
}
```

これによって、読者はコードの動作を確認し、また変更して詳しく知ることができます。そのためには`editable`をコンマ区切りで
コードブロックのはじめに追加してください。

````markdown
```rust,editable
//...ここにコードを書く
```
````

さらに、`ignore`で`mdbook`でビルド、テストをスキップすることができます。

````markdown
```rust,editable,ignore
//...ここにコードを書く
```
````

## docsで使う

[公式Rustドキュメント][official-rust-docs]に「Run」というボタンがついているのに気づいた人もいるかもしれません。
これはサンプルコードをRust Playgroundで新しいタブに開きます。この機能は#[doc]属性の[`html_playground_url`][html-playground-url]で
使うことができます。

### こちらも参照:

- [Rust Playground][rust-playground]
- [開発版playpen][next-gen-playpen]
- [rustdoc Book][rustdoc-book]

[rust-playground]: https://play.rust-lang.org/
[next-gen-playpen]: https://github.com/integer32llc/rust-playground/
[mdbook]: https://github.com/rust-lang/mdBook
[official-rust-docs]: https://doc.rust-lang.org/core/
[rustdoc-book]: https://doc.rust-lang.org/rustdoc/what-is-rustdoc.html
[html-playground-url]: https://doc.rust-lang.org/rustdoc/the-doc-attribute.html#html_playground_url
