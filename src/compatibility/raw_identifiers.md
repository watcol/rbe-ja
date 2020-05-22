# Raw識別子

Rustは他のプログラミング言語のように、「キーワード」を持っています。これは言語
レベルで意味がある単語であるため、変数名や、関数名などには使えません。Raw識別子は
本当は許可されていないキーワードを使うのに使われます。主に、これはRustが新しい
キーワードを提供したが、ライブラリは古いエディションのRustを使っているため、変数名
や関数名とキーワードがかぶってしまうときに使います・

例えば、クレート`foo`がRust 2015を使っていて、`try`という名前の関数を持っているとします。
Rust 2018では、これはキーワードとして使われているため、 Raw識別子なしではこの関数は
使えません。

```rust,ignore
extern crate foo;

fn main() {
    foo::try();
}
```

このようなエラーに遭遇するでしょう:

```text
error: expected identifier, found keyword `try`
 --> src/main.rs:4:4
  |
4 | foo::try();
  |      ^^^ expected identifier, found keyword
```

Raw識別子を使うとこのように書けます。

```rust,ignore
extern crate foo;

fn main() {
    foo::r#try();
}
```
