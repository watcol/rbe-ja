# 型匿名性

クロージャで簡潔にスコープから変数をキャプチャできます。なにか注意する点は
あるのでしょうか? もちろんです。クロージャを引数として取る時、[ジェネリック
][generics]を使わなくてはいけません。

```rust
// `F`はジェネリックである必要がある。
fn apply<F>(f: F) where
    F: FnOnce() {
    f();
}
```

クロージャが作られた時、コンパイラはキャプチャした変数を保存する匿名の
、`Fn`、`FnMut`、`FnOnce`の3つのトレイトの内一つをその型に対して実装した
構造体を作成し、呼び出されるまで待ちます。

この構造体は型が未指定なため、関数での使用にはジェネリックが必要です。
しかし、型パラメータ`<T>`を指定するだけではまだ曖昧です。そのため、`Fn`、
`FnMut`、`FnOnce`のどれを実装しているか明示する必要があります。

```rust,editable
// `F`は`Fn`を実装している必要があり、クロージャは何もとらず何も返さない。
// `print`にはこれが正しい。
fn apply<F>(f: F) where
    F: Fn() {
    f();
}

fn main() {
    let x = 7;

    // `x`を匿名の型としてキャプチャし、それに
    // `Fn`を実装する。これを`print`に格納する。
    let print = || println!("{}", x);

    apply(print);
}
```

### こちらも参照:

- [解析][thorough_analysis]
- [`Fn`][fn]
- [`FnMut`][fn_mut]
- [`FnOnce`][fn_once]

[generics]: ../../generics.md
[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fn_mut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[fn_once]: https://doc.rust-lang.org/std/ops/trait.FnOnce.html
[thorough_analysis]: https://huonw.github.io/blog/2015/05/finding-closure-in-rust/
