# 入力関数

クロージャが引数として使えることから、関数も同じようにできないのかと思った人も
いるかもしれません。その通り、できます! クロージャを引数に取る関数を定義したら、
そのクロージャのトレイト境界を満たすすべての関数を引数として渡すことができます。

```rust,editable
// `Fn`を境界としたジェネリック型`F`を引数に取る
// 関数を定義し、渡されたクロージャを即座に呼び出す。
fn call_me<F: Fn()>(f: F) {
    f();
}

// `Fn`境界を満たす関数を定義する。
fn function() {
    println!("I'm a function!");
}

fn main() {
    // `Fn`境界を満たすクロージャを定義する。
    let closure = || println!("I'm a closure!");

    call_me(closure);
    call_me(function);
}
```

クロージャの捕捉について詳しく見たいときは、`Fn`、`FnMut`、`FnOnce`
トレイトのドキュメントを参照してください。

### こちらも参照:

- [`Fn`][fn]
- [`FnMut`][fn_mut]
- [`FnOnce`][fn_once]

[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fn_mut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[fn_once]: https://doc.rust-lang.org/std/ops/trait.FnOnce.html
