# 構造体

構造体も関数と同じようにアノテーションできます。

```rust,editable
// `Borrowed`は`i32`の参照を保存します。`i32`の参照は`Borrowed`より
// 長生きする必要があります。
#[derive(Debug)]
struct Borrowed<'a>(&'a i32);

// 同じように、両方共構造体より長生きする必要があります。
#[derive(Debug)]
struct NamedBorrowed<'a> {
    x: &'a i32,
    y: &'a i32,
}

// `i32`またはその参照を格納するenum。
#[derive(Debug)]
enum Either<'a> {
    Num(i32),
    Ref(&'a i32),
}

fn main() {
    let x = 18;
    let y = 15;

    let single = Borrowed(&x);
    let double = NamedBorrowed { x: &x, y: &y };
    let reference = Either::Ref(&x);
    let number    = Either::Num(y);

    println!("x is borrowed in {:?}", single);
    println!("x and y are borrowed in {:?}", double);
    println!("x is borrowed in {:?}", reference);
    println!("y is *not* borrowed in {:?}", number);
}
```

### こちらも参照:

- [`struct`][structs]


[structs]: ../../custom_types/structs.md
