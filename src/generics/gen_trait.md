# トレイト

もちろん`トレイト`もジェネリックにできます。自分自身を入力とした
ジェネリックなメソッド`drop`をもつ`Drop`トレイトをジェネリックで
再実装します。

```rust,editable
// コピーできない型
struct Empty;
struct Null;

// `T`に対してジェネリックなトレイト
trait DoubleDrop<T> {
    // 呼び出し元と`T`型の引数をとり、何もしないメソッド。
    fn double_drop(self, _: T);
}

// `DoubleDrop<T>`をジェネリックな引数`T`と
// 呼び出し元`U`に対して実装する。
impl<T, U> DoubleDrop<T> for U {
    // このメソッドは両方の所有権をとり、
    // 両方開放する。
    fn double_drop(self, _: T) {}
}

fn main() {
    let empty = Empty;
    let null  = Null;

    // `empty`と`null`を開放する。
    empty.double_drop(null);

    //empty;
    //null;
    // ^ TODO: これらの行をアンコメントしてみてください
}
```

### こちらも参照:

- [`Drop`][Drop]
- [`struct`][structs]
- [`trait`][traits]

[Drop]: https://doc.rust-lang.org/std/ops/trait.Drop.html
[structs]: ../custom_types/structs.md
[traits]: ../trait.md
