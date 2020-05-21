# `impl Trait`

もし関数が`MyTrait`を実装した型を返す時、返り値を`-> impl MyTrait`と書くことができます。
これによって型指定師をシンプルに書くことができます!

```rust,editable
use std::iter;
use std::vec::IntoIter;

// この関数は2つの`Vec<i32>`を連結し、イテレータを返します。
// 返り値が複雑になっています。
fn combine_vecs_explicit_return_type(
    v: Vec<i32>,
    u: Vec<i32>,
) -> iter::Cycle<iter::Chain<IntoIter<i32>, IntoIter<i32>>> {
    v.into_iter().chain(u.into_iter()).cycle()
}

// 上と同じですが、返り値に`impl Trait`を使っています。
// とてもシンプルになっています。
fn combine_vecs(
    v: Vec<i32>,
    u: Vec<i32>,
) -> impl Iterator<Item=i32> {
    v.into_iter().chain(u.into_iter()).cycle()
}

fn main() {
    let v1 = vec![1, 2, 3];
    let v2 = vec![4, 5];
    let mut v3 = combine_vecs(v1, v2);
    assert_eq!(Some(1), v3.next());
    assert_eq!(Some(2), v3.next());
    assert_eq!(Some(3), v3.next());
    assert_eq!(Some(4), v3.next());
    assert_eq!(Some(5), v3.next());
    println!("all done");
}
```

さらに、Rustのいくつかの型は直接記述できません。例えば、クロージャは、無名な
具象型を持っていて、`impl Trait`構文を使わなければ、ヒープにクロージャを格納して
返す必要がありました。しかし、今は静的にこう書けます。

```rust,editable
// 入力に`y`を加えて返すクロージャをかえす関数
fn make_adder_function(y: i32) -> impl Fn(i32) -> i32 {
    let closure = move |x: i32| { x + y };
    closure
}

fn main() {
    let plus_one = make_adder_function(1);
    assert_eq!(plus_one(2), 3);
}
```

また、`impl Trait`は`map`、`filter`などのクロージャを使ったイテレータを返すのにも
使えます! これによって、`map`や`filter`を簡単に使えるようになります。以前はクロージャ
の型は名前を持たないため、クロージャを使ったイテレータは、明示的に返り値の型が書けなかった
ものが、`impl Trait`を使って簡単に書けるようになったからです。

```rust,editable
fn double_positives<'a>(numbers: &'a Vec<i32>) -> impl Iterator<Item = i32> + 'a {
    numbers
        .iter()
        .filter(|x| x > &&0)
        .map(|x| x * 2)
}
```
