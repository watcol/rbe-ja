# 境界

ジェネリックを使っていると、型パラメータが特定の機能を持っていることを規定するために、
トレイトを*境界*として使うことがあります。例えば、次の例は変数をプリントするため、
型`T`が`Display`トレイトを実装していることを規定しています。つまり、`T`は`Display`
を実装していなければならないのです。

```rust,ignore
// `Display`トレイトを実装している型`T`に対してジェネリックな
// 関数`printer`を定義する。
fn printer<T: Display>(t: T) {
    println!("{}", t);
}
```

境界はジェネリックをすべての型ではなく、あるトレイトを実装している
型だけに対して適用するためにあります。つまり

```rust,ignore
struct S<T: Display>(T);

// エラー! `Vec<T>`は`Display`を実装していません。
// この特殊化は失敗します。
let s = S(vec![1]);
```

境界のもう一つの効果は、境界となるトレイトに実装されているメソッドを使用できることです。
例えば

```rust,editable
// プリントマーカー`{:?}`を実装するトレイト
use std::fmt::Debug;

trait HasArea {
    fn area(&self) -> f64;
}

impl HasArea for Rectangle {
    fn area(&self) -> f64 { self.length * self.height }
}

#[derive(Debug)]
struct Rectangle { length: f64, height: f64 }
#[allow(dead_code)]
struct Triangle  { length: f64, height: f64 }

// ジェネリック型`T`は`Debug`を実装している必要があります。型に関係なく、
// これは動作します。
fn print_debug<T: Debug>(t: &T) {
    println!("{:?}", t);
}

// `T`は`HasArea`を実装している必要があります。`HasArea`のメソッド`area`
// を使用するための境界です。
fn area<T: HasArea>(t: &T) -> f64 { t.area() }

fn main() {
    let rectangle = Rectangle { length: 3.0, height: 4.0 };
    let _triangle = Triangle  { length: 3.0, height: 4.0 };

    print_debug(&rectangle);
    println!("Area: {}", area(&rectangle));

    //print_debug(&_triangle);
    //println!("Area: {}", area(&_triangle));
    // ^ TODO: これらをアンコメントしてみてください。
    // | エラー: `Debug`または`HasArea`を実装していません。
}
```

さらに、読みやすくするため、場合によっては[`where`][where]節も
境界を適用するのに使われます。

### こちらも参照:

- [`std::fmt`][fmt]
- [`struct`s][structs]
- [`trait`s][traits]

[fmt]: ../hello/print.md
[methods]: ../fn/methods.md
[structs]: ../custom_types/structs.md
[traits]: ../trait.md
[where]: ../generics/where.md
