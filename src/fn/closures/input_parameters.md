# 引数として

Rustでその場で変数をキャプチャするときは型注釈を省略しましたが、関数を書くときは
この曖昧さは許されません。クロージャを引数として受け取るときは、いくつかの
`トレイト`を使います。制限の少ない順に、以下の通りです。

* `Fn`: 参照(`&T`)をキャプチャするクロージャ
* `FnMut`: 可変参照(`&mut T`)をキャプチャするクロージャ
* `FnOnce`: 値(`T`)をキャプチャするクロージャ

コンパイラはできる限り制限が最小になるように変数をキャプチャします。

例えば、引数は`FnOnce`を注釈していたとしたら、クロージャは`&T`、`&mut T`、`T`
をキャプチャする可能性がありますが、コンパイラはクロージャがどのように
使われているかに応じてこれを決定します。

なぜなら、値の移動ができる時、どのタイプの借用もできるからです。
その逆はできないことに注意してください。引数が`Fn`を注釈していた場合、
`&mut T`や`T`のキャプチャはできません。

次の例で、`Fn`、`FnMut`、`FnOnce`を入れ替えて何が起こるか試してください。

```rust,editable
// クロージャを引数として取り、呼び出す関数
// <F>はFが「ジェネリック型の引数」であることを表します。
fn apply<F>(f: F) where
    // このクロージャは引数を取らず、何も返しません
    F: FnOnce() {
    // ^ TODO: `Fn`や`FnMat`に変えてみてください。

    f();
}

// クロージャをとり、`i32`をとる関数
fn apply_to_3<F>(f: F) -> i32 where
    // `i32`をとり`i32`を取るクロージャ
    F: Fn(i32) -> i32 {

    f(3)
}

fn main() {
    use std::mem;

    let greeting = "hello";
    // コピーできない型
    // `to_owned`は借用したデータから自分のデータを作成するのに使います。
    let mut farewell = "goodbye".to_owned();

    // `greeting`という参照と`farewell`という値の2つを
    // キャプチャする
    let diary = || {
        // `greeting`は参照なので`Fn`が必要です。
        println!("I said {}.", greeting);  // {}と言った。

        // `farewell`は変更が必要なので可変参照として
        // キャプチャする。ここで`FnMut`が必要になる。
        farewell.push_str("!!!");
        println!("Then I screamed {}.", farewell);  // そして{}と叫んだ。
        println!("Now I can sleep. zzzzz");  // いま寝る。zzzzz

        // `farewell`を手動でDropするため、値としてキャプチャする。
        // ここで`FnOnce`が必要となる。
        mem::drop(farewell);
    };

    // クロージャを実行する関数を呼び出す
    apply(diary);

    // `double`は`apply_to_3`のトレイトの要件を満たす
    let double = |x| 2 * x;

    println!("3 doubled: {}", apply_to_3(double));
}
```

### こちらも参照:

- [`std::mem::drop`][drop]
- [`Fn`][fn]
- [`FnMut`][fnmut]
- [ジェネリック][generics]
- [where][where]
- [`FnOnce`][fnonce]

[drop]: https://doc.rust-lang.org/std/mem/fn.drop.html
[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fnmut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[fnonce]: https://doc.rust-lang.org/std/ops/trait.FnOnce.html
[generics]: ../../generics.md
[where]: ../../generics/where.md
