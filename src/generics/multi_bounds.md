# 複数の境界

`+`で複数の境界が適用できます。複数の引数を受け取るときは、
通常通り`,`で分割します。

```rust,editable
use std::fmt::{Debug, Display};

fn compare_prints<T: Debug + Display>(t: &T) {
    println!("Debug: `{:?}`", t);
    println!("Display: `{}`", t);
}

fn compare_types<T: Debug, U: Debug>(t: &T, u: &U) {
    println!("t: `{:?}`", t);
    println!("u: `{:?}`", u);
}

fn main() {
    let string = "words";
    let array = [1, 2, 3];
    let vec = vec![1, 2, 3];

    compare_prints(&string);
    //compare_prints(&array);
    // TODO ^ この行をアンコメントしてみてください

    compare_types(&array, &vec);
}
```

### こちらも参照:

- [`std::fmt`][fmt]
- [`trait`][traits]

[fmt]: ../hello/print.md
[traits]: ../trait.md
