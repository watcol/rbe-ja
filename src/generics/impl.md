# 実装

関数のように、メソッドの実装に関してもジェネリック型特有の記法が必要です。

```rust
struct S; // 具象型`S`
struct GenericVal<T>(T); // ジェネリック型`GenericVal`

// GenericValを明示的に型パラメータを指定して実装する。
impl GenericVal<f32> {} // `f32`を指定する
impl GenericVal<S> {} // 上で定義した`S`を指定する

// ジェネリックにするには`<T>`が必要です。
impl<T> GenericVal<T> {}
```

```rust,editable
struct Val {
    val: f64,
}

struct GenVal<T> {
    gen_val: T,
}

// impl of Val
impl Val {
    fn value(&self) -> &f64 {
        &self.val
    }
}

// ジェネリック型`T`に対するGenValの実装
impl<T> GenVal<T> {
    fn value(&self) -> &T {
        &self.gen_val
    }
}

fn main() {
    let x = Val { val: 3.0 };
    let y = GenVal { gen_val: 3i32 };

    println!("{}, {}", x.value(), y.value());
}
```

### こちらも参照:

- [参照を返す関数][fn]
- [`impl`][methods]
- [`struct`][structs]


[fn]: ../scope/lifetime/fn.md
[methods]: ../fn/methods.md
[specialization_plans]: https://blog.rust-lang.org/2015/05/11/traits.html#the-future
[structs]: ../custom_types/structs.md
