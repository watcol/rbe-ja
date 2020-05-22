# `Option`と`unwrap`

前の例で、どのようにしてプログラムが失敗したときにストップするかを見て
きました。ここではプリンセスが不適切な贈り物(ヘビ)を受け取ったときに
`panic`を起こしました。しかし、プリンセスが贈り物をもらえなかったときは
どうでしょうか? このケースも良くないため、処理しなければいけません。

Rustでは使えない、空データを指すポインタの代わりに、なにも贈らないことを
空文字列(`""`)を用いて表すこともできます。

しかし、`std`ライブラリの`Option<T>`という`enum`を使えば、何も無いことを
表すことができます。

* `Some(T)`: `T`型の値がある場合
* `None`: なにも値がない場合

これは、`match`で明示的に処理することも、`unwrap`で暗示的に処理することが
できます。暗示的な処理では、その中の値を返し、なければ`panic`を起こします。

[expect][expect]を使えば、`panic`の表示をカスタマイズできますが、`unwrap`は
意味のあまりない出力しか返しません。以下の例では、明示的に処理し、`panic`
よりカスタマイズされた結果を返しています。

```rust,editable,ignore,mdbook-runnable
// 平民は、贈り物をすべて確認して、うまく処理します。
// すべての贈り物は`match`で明示的に処理されています。
fn give_commoner(gift: Option<&str>) {
    // Specify a course of action for each case.
    match gift {
        Some("snake") => println!("Yuck! I'm putting this snake back in the forest."),  // オエーッ! このヘビを森に返してくるわ。
        Some(inner)   => println!("{}? How nice.", inner),  // {}? いいじゃないの。
        None          => println!("No gift? Oh well."),  // 何もない? 仕方ないわ。
    }
}

// 私達のプリンセスは世の中を知らないため、ヘビを見ると`panic`してしまいます。
// すべての贈り物は`unwrap`で暗示的に処理されます。
fn give_princess(gift: Option<&str>) {
    // `unwrap`は`None`を受け取ると`panic`します。
    let inside = gift.unwrap();
    if inside == "snake" { panic!("AAAaaaaa!!!!"); }

    println!("I love {}s!!!!!", inside);
}

fn main() {
    let food  = Some("cabbage");
    let snake = Some("snake");
    let void  = None;

    give_commoner(food);
    give_commoner(snake);
    give_commoner(void);

    let bird = Some("robin");
    let nothing = None;

    give_princess(bird);
    give_princess(nothing);
}
```

[expect]: https://doc.rust-lang.org/std/option/enum.Option.html#method.expect
