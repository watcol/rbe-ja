# if let

enumをマッチする時、しばしば`match`は冗長です。例えば

```rust
// `Option<i32>`型の`optional`を作成する
let optional = Some(7);

match optional {
    Some(i) => {
        println!("This is a really long string and `{:?}`", i);  // これは本当に長い文字列で、`{:?}`です
        // ^ iをoptionから分割代入しているので、2つインデントが必要です。
    },
    _ => {},
    // ^ `match`はすべてを網羅しないといけないため、必要です。
    // 無駄だと思いませんか?
};

```

`if let`はこの場合おいて簡潔な上、失敗時の処理も柔軟にできます。

```rust,editable
fn main() {
    // すべて`Option<i32>`型です
    let number = Some(7);
    let letter: Option<i32> = None;
    let emoticon: Option<i32> = None;

    // `if let`は、「もし`let`が`number`を分解した
    // 結果が`Some(i)`なら、ブロックを実行する。」
    // という意味です。
    if let Some(i) = number {
        println!("Matched {:?}!", i);  // {:?}にマッチしました!
    }

    // もし失敗を扱いたければ、elseを使います。
    if let Some(i) = letter {
        println!("Matched {:?}!", i);
    } else {
        // 分解に失敗した時、このブロックを実行します。
        println!("Didn't match a number. Let's go with a letter!");  // 数字にマッチしませんでした。文字で行きましょう!
    }

    // もう一つ失敗したときの処理を作る
    let i_like_letters = false;

    if let Some(i) = emoticon {
        println!("Matched {:?}!", i);
    // 失敗した時、`else if`の条件を見て、分岐します。
    } else if i_like_letters {
        println!("Didn't match a number. Let's go with a letter!");
    } else {
        // 条件がfalseの時、この枝を実行します。
        println!("I don't like letters. Let's go with an emoticon :)!");  // 文字は嫌いです。絵文字で行きましょう :)
    }
}
```

同じように、`if let`はすべてのenumで使えます。

```rust,editable
// 例で使うenum
enum Foo {
    Bar,
    Baz,
    Qux(u32)
}

fn main() {
    // 例で使う変数
    let a = Foo::Bar;
    let b = Foo::Baz;
    let c = Foo::Qux(100);
    
    // 変数aはFoo::Barにマッチする
    if let Foo::Bar = a {
        println!("a is foobar");
    }
    
    // 変数bはFoo::Barにマッチしないので、
    // 何も出力しない。
    if let Foo::Bar = b {
        println!("b is foobar");
    }
    
    // 変数cは値を持つFoo::Quxにマッチし、
    // 前の例のSome()と同じような挙動をする。
    if let Foo::Qux(value) = c {
        println!("c is {}", value);
    }

    // `if let`でも束縛は使えます。
    if let Foo::Qux(value @ 100) = c {
        println!("c is one hundred");
    }
}
```

もう一つの`if let`の恩恵は、enumのパラメータなしの列挙子にマッチできることです。enumは`PartialEq`を実装も継承もしないため、enumのインスタンスは比較できず、
`if Foo::Bar == a` はコンパイルに失敗しますが、 `if let`で同じことができます。

挑戦しますか? 次の例を`if let`を使うように修正してください。

```rust,editable,ignore,mdbook-runnable
// このenumはPartialEqを実装も継承もしません。
// よって、下のFoo::Bar == aは失敗します。
enum Foo {Bar}

fn main() {
    let a = Foo::Bar;

    // 変数aはFoo::Barにマッチします
    if Foo::Bar == a {
    // ^-- この節はコンパイルエラーを起こします。代わりに`if let`を使ってください。
        println!("a is foobar");
    }
}
```

### こちらも参照:

- [`enum`][enum]
- [`Option`][option]
- [RFC][if_let_rfc]

[enum]: ../custom_types/enum.md
[if_let_rfc]: https://github.com/rust-lang/rfcs/pull/160
[option]: ../std/option.md
