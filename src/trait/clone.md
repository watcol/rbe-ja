# 複製

リソースについて対処する時、デフォルトで代入元や関数呼び出しによって
値が受け渡されます。しかし、ときどきレソースのコピーが作りたい場合が
あります。

[`Clone`][clone]トレイトによって、これを正確に行なえます。一般的なもので
いうと、`.clone()`めそっどは`Clone`トレイトによって定義されています。

```rust,editable
// リソースのない構造体
#[derive(Debug, Clone, Copy)]
struct Unit;

// `Clone`トレイトを持つタプル構造体
#[derive(Clone, Debug)]
struct Pair(Box<i32>, Box<i32>);

fn main() {
    // `Unit`をインスタンス化する
    let unit = Unit;
    // `Unit`をコピーする。ムーブするリソースが何もない。
    let copied_unit = unit;

    // `Unit`は独立して使える。
    println!("original: {:?}", unit);
    println!("copy: {:?}", copied_unit);

    // `Pair`をインスタンス化する。
    let pair = Pair(Box::new(1), Box::new(2));
    println!("original: {:?}", pair);

    // `pair`を`moved_pair`にコピーした時、リソースがムーブする。
    let moved_pair = pair;
    println!("copy: {:?}", moved_pair);

    // エラー! `pair`はリソースを持っていません。
    //println!("original: {:?}", pair);
    // TODO ^ この行をアンコメントしてみてください

    // `moved_pair`から`cloned_pair`に複製する(リソースも)
    let cloned_pair = moved_pair.clone();
    // std::mem::dropでもとのリソースをdropする。
    drop(moved_pair);

    // エラー! `moved_pair`はdropされています。
    //println!("copy: {:?}", moved_pair);
    // TODO ^ この行をアンコメントしてみてください

    // .clone()の結果はまだ使えます!
    println!("clone: {:?}", cloned_pair);
}
```

[clone]: https://doc.rust-lang.org/std/clone/trait.Clone.html
