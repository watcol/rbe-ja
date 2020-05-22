# ドキュメンテーション

`cargo doc`で`target/doc`上にドキュメンテーションを作ることができます。

`cargo test`で(ドキュメンテーションテストを含む)すべてのテストを実行でき、さらに
`cargo test --doc`でドキュメンテーションテストのみ実行できます。

このコマンドは`rustdoc`(や`rustc`)と必要に応じて連携をとります。

## Docコメント

Docコメントはドキュメントが必要な巨大プロジェクトに有用です。`rustdoc`
を実行した時、コメントがドキュメントにコンパイルされます。
これは`///`から始まり、[Markdown]をサポートしています。

````rust,editable,ignore
#![crate_name = "doc"]

/// 人類を表す
pub struct Person {
    /// ジュリエットがどんなに嫌がろうとも、人は必ず名前を持ちます。
    name: String,
}

impl Person {
    /// 与えられた名前を持つPersonを返す。
    ///
    /// # 引数
    ///
    /// * `name` - 人の名前を保持する文字列
    ///
    /// # 例
    ///
    /// ```
    /// // コメント内のコードは実行できます。
    /// // --testを`rustdoc`に渡すと、テストとして機能します!
    /// use doc::Person;
    /// let person = Person::new("name");
    /// ```
    pub fn new(name: &str) -> Person {
        Person {
            name: name.to_string(),
        }
    }

    /// フレンドリーな挨拶!
    ///
    /// `Person`に対して呼び出されたら、"Hello, [name]"と言う。
    pub fn hello(& self) {
        println!("Hello, {}!", self.name);
    }
}

fn main() {
    let john = Person::new("John");

    john.hello();
}
````

テストを実行するためには、コードをライブラリとしてビルドし、テストするライブラリ
がどこにあるのか`rustdoc`に教えて、それぞれのテストとリンクできるようにする。

```shell
$ rustc doc.rs --crate-type lib
$ rustdoc --test --extern doc="libdoc.rlib" doc.rs
```

## Doc属性

以下は、`rustdoc`で使われる`#[doc]`属性についての例です。

### `inline`

分けられたページへのリンクの代わりにインラインドキュメント
を使う。

```rust,ignore
#[doc(inline)]
pub use bar::Bar;

/// barドキュメント
mod bar {
    /// Barのドキュメント
    pub struct Bar;
}
```

### `no_inline`

別のページからリンクすることを宣言する

```rust,ignore
// Example from libcore/prelude
#[doc(no_inline)]
pub use crate::mem::drop;
```

### `hidden`

これをドキュメント煮含めないことを`rustdoc`に教える

```rust,editable,ignore
// Example from the futures-rs library
#[doc(hidden)]
pub use self::async_await::*;
```

ドキュメンテーション用に、`rustdoc`はコミュニティによって広く使われています。これは[標準ライブラリのドキュメント
](https://doc.rust-lang.org/std/)にも使われています。

### こちらも参照:

- [The Rust Book: 有用なドキュメンテーションコメントを作る][book]
- [The rustdoc Book][rustdoc-book]
- [The Reference: Doc comments][ref-comments]
- [RFC 1574: API Documentation Conventions][api-conv]
- [RFC 1946: Relative links to other items from doc comments (intra-rustdoc links)][intra-links]
- [Is there any documentation style guide for comments? (reddit)][reddit]

[markdown]: https://en.wikipedia.org/wiki/Markdown
[book]: https://doc.rust-lang.org/book/ch14-02-publishing-to-crates-io.html#making-useful-documentation-comments
[ref-comments]: https://doc.rust-lang.org/stable/reference/comments.html#doc-comments
[rustdoc-book]: https://doc.rust-lang.org/rustdoc/index.html
[api-conv]: https://rust-lang.github.io/rfcs/1574-more-api-documentation-conventions.html#appendix-a-full-conventions-text
[intra-links]: https://rust-lang.github.io/rfcs/1946-intra-rustdoc-links.html
[reddit]: https://www.reddit.com/r/rust/comments/ahb50s/is_there_any_documentation_style_guide_for/
