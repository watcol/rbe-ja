# テストケース: 連結リスト

`enums`は連結リストを作るのに適当です。

```rust,editable
use crate::List::*;

enum List {
    // Cons: 要素と次のノードを指すポインタからなるタプル構造体
    Cons(u32, Box<List>),
    // Nil: 連結リストの終わりを指すノード
    Nil,
}

// enumに関連付けられた関数
impl List {
    // 空のリストを作る。
    fn new() -> List {
        // `Nil`は`List`型を持つ
        Nil
    }

    // リストを受け取り、前に要素を追加したものを返す。
    fn prepend(self, elem: u32) -> List {
        // `Cons`も`List`型を持つ。
        Cons(elem, Box::new(self))
    }

    // Listの長さを返す
    fn len(&self) -> u32 {
        // このメソッドの振る舞いは`self`の列挙子によって変化するため、
        // matchを使う必要があります。
        // `self`の型は`&List`なので、`*self`の型は`List`になる。match
        // するときは参照`&T`より実体`T`を使う方が好ましい。
        match *self {
            // `self`は借用なので、tailの所有権は取れない。
            // 代わりにtailの参照を取る。
            Cons(_, ref tail) => 1 + tail.len(),
            // 空リストの長さは0
            Nil => 0
        }
    }

    // Listを(ヒープ上の)文字列として返す。
    fn stringify(&self) -> String {
        match *self {
            Cons(head, ref tail) => {
                // `format!`は`print!`に似ているが、コンソールに出力せず、
                // ヒープ上の文字列を返す。
                format!("{}, {}", head, tail.stringify())
            },
            Nil => {
                format!("Nil")
            },
        }
    }
}

fn main() {
    // 空リストを作る
    let mut list = List::new();

    // 要素をいくつか追加する
    list = list.prepend(1);
    list = list.prepend(2);
    list = list.prepend(3);

    // 最終的なリストの状態を見る
    println!("linked list has length: {}", list.len());
    println!("{}", list.stringify());
}
```

### こちらも参照:

[`Box`][box]と[メソッド][methods]

[box]: ../../std/box.md
[methods]: ../../fn/methods.md
