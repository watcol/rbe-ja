# コンビネータ: `and_then`

`map()`は`match`文を連結してシンプルにするものでした。しかし、
`Option<T>`を返す関数で`map()`を使えば、ネストされた`Option<Option<t>>`
が必要になります。この場合、複数の呼び出しを連続で使うと混乱を招きます。
ここで、他の言語では`flatmap`として知られている、別のコンビネータ
`and_then()`の登場です。

`and_then()`は引数として渡された関数にラップされた値を返しますが、それが`None`なら、
`None`を返します。

In the following example, `cookable_v2()` results in an `Option<Food>`. 
Using `map()` instead of `and_then()` would have given an 
`Option<Option<Food>>`, which is an invalid type for `eat()`.

```rust,editable
#![allow(dead_code)]

#[derive(Debug)] enum Food { CordonBleu, Steak, Sushi }
#[derive(Debug)] enum Day { Monday, Tuesday, Wednesday }

// 寿司の材料を持っていません。
fn have_ingredients(food: Food) -> Option<Food> {
    match food {
        Food::Sushi => None,
        _           => Some(food),
    }
}

// コルドン・ブルー(訳注: カツレツのようなもの)のレシピを持っていません。
fn have_recipe(food: Food) -> Option<Food> {
    match food {
        Food::CordonBleu => None,
        _                => Some(food),
    }
}

// 料理を作るには、材料とレシピの両方が必要です。
// `match`でロジックの流れを表す。
fn cookable_v1(food: Food) -> Option<Food> {
    match have_recipe(food) {
        None       => None,
        Some(food) => match have_ingredients(food) {
            None       => None,
            Some(food) => Some(food),
        },
    }
}

// `and_then()`でコンパクトに書き直すことができます。
fn cookable_v2(food: Food) -> Option<Food> {
    have_recipe(food).and_then(have_ingredients)
}

fn eat(food: Food, day: Day) {
    match cookable_v2(food) {
        Some(food) => println!("Yay! On {:?} we get to eat {:?}.", day, food),
        None       => println!("Oh no. We don't get to eat on {:?}?", day),
    }
}

fn main() {
    let (cordon_bleu, steak, sushi) = (Food::CordonBleu, Food::Steak, Food::Sushi);

    eat(cordon_bleu, Day::Monday);
    eat(steak, Day::Tuesday);
    eat(sushi, Day::Wednesday);
}
```

### こちらも参照:

- [クロージャ][closures]
- [`Option`][option]
- [`Option::and_then()`][and_then]

[closures]: ../../fn/closures.md
[option]: https://doc.rust-lang.org/std/option/enum.Option.html
[and_then]: https://doc.rust-lang.org/std/option/enum.Option.html#method.and_then
