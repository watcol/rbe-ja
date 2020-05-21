# 演算子オーバーロード

Rustでは、ほとんどの演算子がトレイトによってオーバーロードできます。つまり、いくつかの演算子は
引数によって違う操作を行うことができます。これは、演算子がメソッド呼び出しの糖衣構文だからこそ
実現できます。例えば、`a + b`の`+`演算子は`add`メソッドを`a.add(b)`のように呼び出します。
この`add`メソッドは`Add`トレイトによって定義されていないため、`+`演算子は`Add`トレイトを
実装すれば使えるようになります。

`Add`のような、演算子オーバーロードに使うトレイトの一覧は[`core::ops`][ops]にあります。

```rust,editable
use std::ops;

struct Foo;
struct Bar;

#[derive(Debug)]
struct FooBar;

#[derive(Debug)]
struct BarFoo;

// `std::ops::Add`トレイトは`+`演算子の動作を定義するのに使います。
// ここでは、右辺値が`Bar`のときの足し算を定義する`Add<Bar>`を実装します。
// このブロックはFoo + Bar = FooBarを実装しています。
impl ops::Add<Bar> for Foo {
    type Output = FooBar;

    fn add(self, _rhs: Bar) -> FooBar {
        println!("> Foo.add(Bar) was called");

        FooBar
    }
}

// 型を逆にすることで、交換法則に従わない実装を作ることができます。
// ここでは、右辺値が`Foo`のときの足し算を定義する`Add<Foo>`を実装します。
// このブロックはBar + Foo = BarFooを定義しています。
impl ops::Add<Foo> for Bar {
    type Output = BarFoo;

    fn add(self, _rhs: Foo) -> BarFoo {
        println!("> Bar.add(Foo) was called");

        BarFoo
    }
}

fn main() {
    println!("Foo + Bar = {:?}", Foo + Bar);
    println!("Bar + Foo = {:?}", Bar + Foo);
}
```

### こちらも参照

- [Add][add]
- [構文インデックス][syntax]

[add]: https://doc.rust-lang.org/core/ops/trait.Add.html
[ops]: https://doc.rust-lang.org/core/ops/
[syntax]:https://doc.rust-lang.org/book/appendix-02-operators.html
