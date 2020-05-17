# CライクなEnum

`enum`はCライクにも使えます。

```rust,editable
// 使用されていないコードの警告をなくす属性
#![allow(dead_code)]

// 値を明示しない場合、0から順に入る。
enum Number {
    Zero,
    One,
    Two,
}

// 明確な指定をするenum
enum Color {
    Red = 0xff0000,
    Green = 0x00ff00,
    Blue = 0x0000ff,
}

fn main() {
    // `enums`は整数にキャストできる。
    println!("zero is {}", Number::Zero as i32);
    println!("one is {}", Number::One as i32);

    println!("roses are #{:06x}", Color::Red as i32);  // バラは#{:06x}
    println!("violets are #{:06x}", Color::Blue as i32);  // スミレは#{:06x}
}
```

### こちらも参照:

[キャスト][cast]

[cast]: ../../types/cast.md
