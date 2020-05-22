# コンビネータ: `map`

`match`は`Option`を処理するのに使えます。しかし、特に引数の値が有効である必要がある
場合、これが億劫だと思うかもしれません。このような場合、[コンビネータ][combinators]
でモジュール的に制御フローを管理することができます。

`Option`は、`Some -> Some`や`None -> None`のような単純な処理を扱うために`map()`という
ビルトインメソッドを持っていて、これを柔軟に連結することができます。

以下の例では、`process()`は以前の関数のすべてをコンパクトに置き換えることができます。

```rust,editable
#![allow(dead_code)]

#[derive(Debug)] enum Food { Apple, Carrot, Potato }

#[derive(Debug)] struct Peeled(Food);
#[derive(Debug)] struct Chopped(Food);
#[derive(Debug)] struct Cooked(Food);

// 食べ物の皮を剥く。もし何もなければ、`None`を返す。
// その他の場合は、剥いた食べ物を返す。
fn peel(food: Option<Food>) -> Option<Peeled> {
    match food {
        Some(food) => Some(Peeled(food)),
        None       => None,
    }
}

// 食べ物を切る。もし何もなければ、`None`を返す。
// その他の場合は、切った食べ物を返す。
fn chop(peeled: Option<Peeled>) -> Option<Chopped> {
    match peeled {
        Some(Peeled(food)) => Some(Chopped(food)),
        None               => None,
    }
}

// 食べ物を料理する。ここでは`match`の代わりに`map()`で処理する。
fn cook(chopped: Option<Chopped>) -> Option<Cooked> {
    chopped.map(|Chopped(food)| Cooked(food))
}

// 食べ物を連続で剥き、切り、料理する。
// `map()`を連結してシンプルなコードを実現している。
fn process(food: Option<Food>) -> Option<Cooked> {
    food.map(|f| Peeled(f))
        .map(|Peeled(f)| Chopped(f))
        .map(|Chopped(f)| Cooked(f))
}

// 食べる前に食べ物があるか確認する必要があります!
fn eat(food: Option<Cooked>) {
    match food {
        Some(food) => println!("Mmm. I love {:?}", food),
        None       => println!("Oh no! It wasn't edible."),
    }
}

fn main() {
    let apple = Some(Food::Apple);
    let carrot = Some(Food::Carrot);
    let potato = None;

    let cooked_apple = cook(chop(peel(apple)));
    let cooked_carrot = cook(chop(peel(carrot)));
    // `process()`でシンプルに調理してみましょう
    let cooked_potato = process(potato);

    eat(cooked_apple);
    eat(cooked_carrot);
    eat(cooked_potato);
}
```

### こちらも参照:

- [クロージャ][closures]
- [`Option`][option]
- [`Option::map()`][map]

[combinators]: https://doc.rust-lang.org/book/glossary.html#combinators
[closures]: ../../fn/closures.md
[option]: https://doc.rust-lang.org/std/option/enum.Option.html
[map]: https://doc.rust-lang.org/std/option/enum.Option.html#method.map
