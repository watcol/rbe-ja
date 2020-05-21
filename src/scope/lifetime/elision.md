# 省略

圧倒的によく使われるライフタイムパターンについては、可読性のために省略
することができます。省略は、単にそれが一般的であるため、存在しています。

次のコードではいくつかの省略の例を紹介します。もっと厳密な記述については、
the bookの[lifetime elision][elision]を参照してください。

```rust,editable
// `elided_input`と`annotated_input`は同じ意味で、`elided_input`の
// ライフタイムはコンパイラによって推論されています。
fn elided_input(x: &i32) {
    println!("`elided_input`: {}", x);
}

fn annotated_input<'a>(x: &'a i32) {
    println!("`annotated_input`: {}", x);
}

// 同様に、`elided_pass`と`annotated_pass`は同じで、
// `elided_pass`では暗示的に記述が加えられているだけです。
fn elided_pass(x: &i32) -> &i32 { x }

fn annotated_pass<'a>(x: &'a i32) -> &'a i32 { x }

fn main() {
    let x = 3;

    elided_input(&x);
    annotated_input(&x);

    println!("`elided_pass`: {}", elided_pass(&x));
    println!("`annotated_pass`: {}", annotated_pass(&x));
}
```

### こちらも参照:

- [省略][elision]

[elision]: https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-elision
