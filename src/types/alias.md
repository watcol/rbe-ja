# エイリアシング

`type`文は存在する型に新しい名前を付けるのに使えます。型は`UpperCamelCase`
(単語のはじめが大文字)の名前を持つ必要があり、そうでないとコンパイラは警告
を出します。ただし、`usize`、`f32`などのプリミティブ型は例外です。

```rust,editable
// `NanoSecond`は`u64`の新しい名前です。
type NanoSecond = u64;
type Inch = u64;

// 警告が出ないように属性を付けます。
#[allow(non_camel_case_types)]
type u64_t = u64;
// TODO ^ 属性を消してみてください。

fn main() {
    // `NanoSecond` = `Inch` = `u64_t` = `u64`.
    let nanoseconds: NanoSecond = 5 as u64_t;
    let inches: Inch = 2 as u64_t;

    // 型エイリアスは新しい型ではないので、余分な型安全性を提供しない
    // ことに注意してください。
    println!("{} nanoseconds + {} inches = {} unit?",
             nanoseconds,
             inches,
             nanoseconds + inches);
}
```

エイリアスは主に冗長性を軽減するために使われます。例えば、`IoResult<T>`型は
`Result<T, IoError>`型のエイリアスです。

### こちらも参照

- [属性](../attribute.md)
