# デバッグ

`std::fmt`のフォーマット用の`トレイト`を使用したい型は、すべてプリント可能
なように実装されている必要があります。`std`ライブラリにあるような型にしか
実装は提供されていません。他は手動で実装*しなければいけません*。

`fmt::Debug`という`トレイト`でこれを簡略化できます。*すべての*型は
`fmt::Debug`の実装を`derive`(自動で作成)できるからです。ただし、
`fmt::Display`の場合は手動で実装しなければいけません。

```rust
// この構造体は`fmt::Display`でも
// `fmt::Debug`でもプリントできません。
struct UnPrintable(i32);

// `derive`属性は、自動的にこの`struct`を`fmt::Debug`で
// プリントできるように実装を作成します。
#[derive(Debug)]
struct DebugPrintable(i32);
```

すべての`std`ライブラリの型も自動的に`{:?}`でプリントできます。

```rust,editable
// `fmt::Debug`の実装を`Structure`にderiveする. `Structure`
// は一つの`i32`をメンバに持っています。
#[derive(Debug)]
struct Structure(i32);

// `Structure`を中に持つ構造体`Deep`を作る。 これもプリント可能
// にする。
#[derive(Debug)]
struct Deep(Structure);

fn main() {
    // `{:?}`は`{}`と似たようなものです。
    println!("{:?} months in a year.", 12);
    println!("{1:?} {0:?} is the {actor:?} name.",
             "Slater",
             "Christian",
             actor="actor's");

    // `Structure`はプリントできます!
    println!("Now {:?} will print!", Structure(3));
    
    // `derive`の問題点は、結果の見た目を制御できない点です。
    // 出力を`7`だけにするにはどうすればよいでしょう?
    println!("Now {:?} will print!", Deep(Structure(7)));
}
```

`fmt::Debug`は確実にプリントを可能にしてくれるのですが、ある意味で美しさを
犠牲にしています。Rustは`{:#?}`による「美しいプリント」も提供しています。

```rust,editable
#[derive(Debug)]
struct Person<'a> {
    name: &'a str,
    age: u8
}

fn main() {
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age };

    // 美しいプリント
    println!("{:#?}", peter);
}
```

出力をコントロールするために手動で`fmt::Display`を実装することもできます。

### こちらも参照:

[属性][attributes], [`derive`][derive], [`std::fmt`][fmt],
そして[`struct`][structs]

[attributes]: https://doc.rust-lang.org/reference/attributes.html
[derive]: ../../trait/derive.md
[fmt]: https://doc.rust-lang.org/std/fmt/
[structs]: ../../custom_types/structs.md

