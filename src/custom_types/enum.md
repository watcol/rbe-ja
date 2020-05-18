# Enum

`enum`はいくつかの型から1つ選ぶような型を作るときに使う。`構造体`の定義を満たすものは
すべて`enum`内で使える。

```rust,editable
//  Webのイベントを分類するのに`enum`を使う。名前と型情報を
// 合わせて列挙子を定義することに注意してください。
// `PageLoad != PageUnload`で、`KeyPress(char) != Paste(String)`です。
// それぞれは異なっていて、独立です。
enum WebEvent {
    // `enum`は`unit`や、
    PageLoad,
    PageUnload,
    // タプル構造体や、
    KeyPress(char),
    Paste(String),
    // cライク構造体のようにできます。
    Click { x: i64, y: i64 },
}

// `WebEvent` enumを引数としてとり、何も返さない関数。
fn inspect(event: WebEvent) {
    match event {
        WebEvent::PageLoad => println!("page loaded"),
        WebEvent::PageUnload => println!("page unloaded"),
        // `enum`内の`c`を分割代入する。
        WebEvent::KeyPress(c) => println!("pressed '{}'.", c),
        WebEvent::Paste(s) => println!("pasted \"{}\".", s),
        // `Click`を`x`と`y`に分割代入する。
        WebEvent::Click { x, y } => {
            println!("clicked at x={}, y={}.", x, y);
        },
    }
}

fn main() {
    let pressed = WebEvent::KeyPress('x');
    // `to_owned()`は文字列のスライスから所有権を持つ`String`を作成します。
    let pasted  = WebEvent::Paste("my text".to_owned());
    let click   = WebEvent::Click { x: 20, y: 80 };
    let load    = WebEvent::PageLoad;
    let unload  = WebEvent::PageUnload;

    inspect(pressed);
    inspect(pasted);
    inspect(click);
    inspect(load);
    inspect(unload);
}

```

## 型エイリアス

型エイリアスを使うと、そのエイリアスからenumの列挙子を参照できます。
これはenumの名前が長過ぎたり、汎用的すぎたりして、名前を変えたいときに
使えるかもしれません。

```rust,editable
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

// 型エイリアスを作る
type Operations = VeryVerboseEnumOfThingsToDoWithNumbers;

fn main() {
    // エイリアスを通してenumを参照できます。
    // 長くも不便でもないです。
    let x = Operations::Add;
}
```

これの最も一般的な使い道は、`impl`ブロック内で`Self`エイリアスとして使われるときです。

```rust,editable
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

impl VeryVerboseEnumOfThingsToDoWithNumbers {
    fn run(&self, x: i32, y: i32) -> i32 {
        match self {
            Self::Add => x + y,
            Self::Subtract => x - y,
        }
    }
}
```

enumと型エイリアスについてもっと知りたければ、この機能がRustの安定版に導入されたときの
[安定化レポート][aliasreport]を読んでください。

### こちらも参照:

- [`match`][match]
- [`fn`][fn]
- [`String`][str]
- ["Type alias enum variants" RFC][type_alias_rfc]

[c_struct]: https://en.wikipedia.org/wiki/Struct_(C_programming_language)
[match]: ../flow_control/match.md
[fn]: ../fn.md
[str]: ../std/str.md
[aliasreport]: https://github.com/rust-lang/rust/pull/61682/#issuecomment-502472847
[type_alias_rfc]: https://rust-lang.github.io/rfcs/2338-type-alias-enum-variants.html
