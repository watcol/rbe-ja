# 関連型

「関連型」を使えば、コンテナ型の中の要素を*出力型*として書くことができるため、
可読性が上げられます。トレイトを定義する構文は以下の通りです。

```rust
// `A`と`B`はトレイト内で`type`キーワードを使って定義できる。
// (注意: この文脈での`type`は型エイリアスを作る`type`とは別物です。)
trait Contains {
    type A;
    type B;

    // これらの型をジェネリックに使うため、構文を変える。
    fn contains(&self, &Self::A, &Self::B) -> bool;
}
```

`Contains`トレイトを使用する時に、`A`と`B`を明示することは
もはや必要ありません。

```rust,ignore
// 関連型を使わない時
fn difference<A, B, C>(container: &C) -> i32 where
    C: Contains<A, B> { ... }

// 関連型を使った時
fn difference<C: Contains>(container: &C) -> i32 { ... }
```

前の節の例を関連型を使って書き直しましょう。

```rust,editable
struct Container(i32, i32);

// コンテナが2つの要素を持つことをチェックするトレイト
// 最初と最後の要素も取得できます。
trait Contains {
    // メソッドで使われるジェネリック型を定義する
    type A;
    type B;

    fn contains(&self, _: &Self::A, _: &Self::B) -> bool;
    fn first(&self) -> i32;
    fn last(&self) -> i32;
}

impl Contains for Container {
    // `A`と`B`の型を定義します。もし入力型が`Container(i32, i32)`なら、
    // 出力型は`i32`と`i32`になります。
    type A = i32;
    type B = i32;

    // `&Self::A`と`&Self::B`も利用可能です。
    fn contains(&self, number_1: &i32, number_2: &i32) -> bool {
        (&self.0 == number_1) && (&self.1 == number_2)
    }
    // 最初の値を取得する
    fn first(&self) -> i32 { self.0 }

    // 最後の値を取得する
    fn last(&self) -> i32 { self.1 }
}

fn difference<C: Contains>(container: &C) -> i32 {
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
