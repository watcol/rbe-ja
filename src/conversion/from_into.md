# `From`と`Into`

[`From`]トレイトと[`Into`]トレイとは本質的にリンクされていて、これは実際には
その実装の一部です。もし型Aから型Bに変換できたら、型Bから型Aに変換できると
信じるのは簡単でしょう。

## `From`

[`From`]トレイトで、他の型からどのように自分自身を作ることができるかを定義できます。
これによって、いくつかの型と変換するとてもシンプルなメカニズムを提供しています。
標準ライブラリに、一般的な型とプリミティブ型を変換するための、このトレイトの
数多くの実装があります。

例として、`str`は`String`に簡単に変換できます。

```rust
let my_str = "hello";
let my_string = String::from(my_str);
```

同じように、独自の型に対して実装できます。

```rust,editable
use std::convert::From;

#[derive(Debug)]
struct Number {
    value: i32,
}

impl From<i32> for Number {
    fn from(item: i32) -> Self {
        Number { value: item }
    }
}

fn main() {
    let num = Number::from(30);
    println!("My number is {:?}", num);
}
```

## `Into`

[`Into`]トレイトは単純に`From`トレイトの逆です。もし`From`トレイトが
あなたの型に実装されていたら、`Into`は必要に応じてこれを呼び出します。

コンパイラはほとんどの場合変換する型を決めることができないので、`Into`
トレイトを使うには、それを指定する必要があります。しかし、これは自由に
機能が得られることを考えれば小さなトレードオフです。

```rust,editable
use std::convert::From;

#[derive(Debug)]
struct Number {
    value: i32,
}

impl From<i32> for Number {
    fn from(item: i32) -> Self {
        Number { value: item }
    }
}

fn main() {
    let int = 5;
    // 型の宣言を削除してみてください
    let num: Number = int.into();
    println!("My number is {:?}", num);
}
```

[`From`]: https://doc.rust-lang.org/std/convert/trait.From.html
[`Into`]: https://doc.rust-lang.org/std/convert/trait.Into.html
