# Where節

境界は`where`節を`{`の直前に置くことでも表現できます。
さらに、`where`節は境界を型パラメータだけでなく、任意の型に
適用することもできます。

場合によっては`where`節は有用です。

* ジェネリック型の指定と境界の指定を明確に分けたい時

```rust,ignore
impl <A: TraitB + TraitC, D: TraitE + TraitF> MyTrait<A, D> for YourType {}

// `where`節で境界を表現する
impl <A, D> MyTrait<A, D> for YourType where
    A: TraitB + TraitC,
    D: TraitE + TraitF {}
```

* `where`節の方が通常の構文より表現力が高いときがあります。
この例の`impl`は`where`節を使わないと表現できません。

```rust,editable
use std::fmt::Debug;

trait PrintInOption {
    fn print_in_option(self);
}

// この例は他に`T: Debug`を使うか他の直接的でない方法を使うしかありません。
// これには`where`節が必要です。
impl<T> PrintInOption for T where
    Option<T>: Debug {
    // プリントされるものが`Option<T>`型なので、`Option<T>: Debug`に
    // 境界を設定したい。他の方法では間違った境界しか設定できない。
    fn print_in_option(self) {
        println!("{:?}", Some(self));
    }
}

fn main() {
    let vec = vec![1, 2, 3];

    vec.print_in_option();
}
```

### こちらも参照:

- [RFC][where]
- [`struct`][struct]
- [`trait`][trait]

[struct]: ../custom_types/structs.md
[trait]: ../trait.md
[where]: https://github.com/rust-lang/rfcs/blob/master/text/0135-where.md
