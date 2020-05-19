# 返り値として

クロージャを引数として取ることができるのなら、クロージャを返す
こともできるはずです。しかし、匿名なクロージャの型は、定義時点では
不明です。そのため、クロージャを返すには`impl Trait`を使う必要が
あります。

クロージャを返すのに使われるトレイトは以下の通りです。

* `Fn`
* `FnMut`
* `FnOnce`

キャプチャしている値をすべて渡すため、`move`キーワードが必要です。これがないと
変数は関数終了と同時にdropされ、クロージャ内のキャプチャしている参照が無効に
なります。

```rust,editable
fn create_fn() -> impl Fn() {
    let text = "Fn".to_owned();

    move || println!("This is a: {}", text)
}

fn create_fnmut() -> impl FnMut() {
    let text = "FnMut".to_owned();

    move || println!("This is a: {}", text)
}

fn create_fnonce() -> impl FnOnce() {
    let text = "FnOnce".to_owned();

    move || println!("This is a: {}", text)
}

fn main() {
    let fn_plain = create_fn();
    let mut fn_mut = create_fnmut();
    let fn_once = create_fnonce();

    fn_plain();
    fn_mut();
    fn_once();
}
```

### こちらも参照:

- [`Fn`][fn]
- [`FnMut`][fnmut]
- [ジェネリック][generics]
- [impl Trait][impltrait]

[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fnmut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[generics]: ../../generics.md
[impltrait]: ../../trait/impl_trait.md
