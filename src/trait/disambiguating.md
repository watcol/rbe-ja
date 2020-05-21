# 重複するトレイトの明確化

一つの型には多くのトレイトが実装できます。2つのトレイトが同じ名前を持つ場合はどうでしょう? 例えば、多くのトレイトが`get()`メソッドを持っているかもしれません。
そして、それらは違う型を返すかもしれません!

それぞれのトレイトが独自の`impl`ブロックを使って宣言するため、
どのトレイトの`get`メソッドなのか明確にわかります。

それを_呼び出す_ ときはどうでしょうか? それらを明確化するために、
完全修飾構文(Fully Qualified Syntax)を使います。

```rust,editable
trait UsernameWidget {
    // そのウィジェットで指定されたユーザー名を返す
    fn get(&self) -> String;
}

trait AgeWidget {
    // そのウィジェットで指定された年齢を返す
    fn get(&self) -> u8;
}

// UsernameWidgetとAgeWidgetを両方実装したフォーム
struct Form {
    username: String,
    age: u8,
}

impl UsernameWidget for Form {
    fn get(&self) -> String {
        self.username.clone()
    }
}

impl AgeWidget for Form {
    fn get(&self) -> u8 {
        self.age
    }
}

fn main() {
    let form = Form{
        username: "rustacean".to_owned(),
        age: 28,
    };

    // これをアンコメントすると、「複数の`get`が見つかりました」というエラー
    // を返します。なぜなら、`get`という名前のメソッドが複数あるためです。
    // println!("{}", form.get());

    let username = <Form as UsernameWidget>::get(&form);
    assert_eq!("rustacean".to_owned(), username);
    let age = <Form as AgeWidget>::get(&form);
    assert_eq!(28, age);
}
```

### こちらも参照:

[The Rust Programming Languageの完全修飾構文の節][trpl_fqsyntax]

[trpl_fqsyntax]: https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#fully-qualified-syntax-for-disambiguation-calling-methods-with-the-same-name
