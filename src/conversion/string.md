# Stringsから変換、Stringに変換

## Stringに変換

ある型から`String`に変換するには、その型に[`ToString`]トレイトを実装するだけで
良いです。 直接実装するより、[`print!`][print]節で説明したように、[`ToString`]を
自動的に提供する[`fmt::Display`][Display]トレイトを実装した方が良いです。

```rust,editable
use std::fmt;

struct Circle {
    radius: i32
}

impl fmt::Display for Circle {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "Circle of radius {}", self.radius)
    }
}

fn main() {
    let circle = Circle { radius: 6 };
    println!("{}", circle.to_string());
}
```

## Stringを解析する

文字列から変換される最も一般的な型は数値型です。慣用的なアプローチは、
[`parse`]関数を使い、型推論か「ターボフィッシュ(turbofish)」構文を
使って型を指定することです。両方の例が以下に紹介されています。

これで文字列をその型に[`FromStr`]トレイトが実装されている限り、指定した型に変換することが
できます。これは標準ライブラリ内の数多くの型で実装できます。この機能をユーザー定義型に
追加するには、単純に[`FromStr`]トレイトを実装すればよいだけです。

```rust,editable
fn main() {
    let parsed: i32 = "5".parse().unwrap();
    let turbo_parsed = "10".parse::<i32>().unwrap();

    let sum = parsed + turbo_parsed;
    println!("Sum: {:?}", sum);
}
```

[`ToString`]: https://doc.rust-lang.org/std/string/trait.ToString.html
[Display]: https://doc.rust-lang.org/std/fmt/trait.Display.html
[print]: ../hello/print.md
[`parse`]: https://doc.rust-lang.org/std/primitive.str.html#method.parse
[`FromStr`]: https://doc.rust-lang.org/std/str/trait.FromStr.html
