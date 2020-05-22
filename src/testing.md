# テスト

Rustはプログラムの正確性に重点をおくプログラミング言語であるため、
言語自体にテストを書く仕組みを備えています。

テストはこの3つのスタイルからなります:

* [ユニット][unit]テスト
* [ドキュメンテーション][doc]テスト
* [整合性][integration]テスト

さらに、テストのための依存を管理する仕組みも備えています:

* [Dev-dependencies][dev-dependencies]

## こちらも参照

* [The Book][doc-testing]のテストの章
* [API Guidelines][doc-nursery]のdoc-testの章

[unit]: testing/unit_testing.md
[doc]: testing/doc_testing.md
[integration]: testing/integration_testing.md
[dev-dependencies]: testing/dev_dependencies.md
[doc-testing]: https://doc.rust-lang.org/book/ch11-00-testing.html
[doc-nursery]: https://rust-lang-nursery.github.io/api-guidelines/documentation.html
