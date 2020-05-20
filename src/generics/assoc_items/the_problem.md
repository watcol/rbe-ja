# 問題

コンテナ型に、その要素に対してジェネリックなトレイトを実装した時、そのトレイトを
使用する人は、すべてのジェネリック型を明記する必要があります。

以下の例では、`Contains`トレイトはジェネリック型AとBの使用を許可しています。
このトレイトは、その後`Container`型に対して実装されていますが、その際、後で
`fn difference()`が使用できるように、`A`と`B`が`i32`であると明記しています。

`Contains`はジェネリックであるため、明示的に*すべて*のジェネリック型を`fn difference()`
にする必要があります。しかし、`C`によって`A`と`B`は決定できるはずです。次の節で紹介する
関連型で、この冗長さを解消できます。

```rust,editable
struct Container(i32, i32);

// 2つの要素がコンテナの中にあることをチェックするトレイト
// 最初と最後の値を取得することもできる。
trait Contains<A, B> {
    fn contains(&self, _: &A, _: &B) -> bool; // 明示的に`A`と`B`が必要。
    fn first(&self) -> i32; // `A`と`B`は不要。
    fn last(&self) -> i32;  // `A`と`B`は不要。
}

impl Contains<i32, i32> for Container {
    // 保存されている値が等しければtrueを返す
    fn contains(&self, number_1: &i32, number_2: &i32) -> bool {
        (&self.0 == number_1) && (&self.1 == number_2)
    }

    // 最初の値を返す。
    fn first(&self) -> i32 { self.0 }

    // 最後の値を返す。
    fn last(&self) -> i32 { self.1 }
}

// `C`は`A`を`B`含んでいるため、`A`と`B`の記述は冗長。
fn difference<A, B, C>(container: &C) -> i32 where
    C: Contains<A, B> {
    container.last() - container.first()
}

fn main() {
    let number_1 = 3;
    let number_2 = 10;

    let container = Container(number_1, number_2);

    println!("Does container contain {} and {}: {}",
        &number_1, &number_2,
        container.contains(&number_1, &number_2));
    println!("First number: {}", container.first());
    println!("Last number: {}", container.last());

    println!("The difference is: {}", difference(&container));
}
```

### こちらも参照:

- [`struct`][structs]
- [`trait`s][traits]

[structs]: ../../custom_types/structs.md
[traits]: ../../trait.md
