# New Typeイディオム

`newtype`イディオムは正しい型の値が供給されることをコンパイル時に保証します。

例えば、年齢確認プログラムでは、`Years`型の値が関数に渡される必要があります。

> 訳注: わかりにくかったので補足すると、年を表す数値も日数を表す数値も`i64`
> ですが、だからといって年齢確認用の関数に日数を渡されても困ります。なので、
> 新しい型を作って数値の意味を明示する、ということです。

```rust, editable
struct Years(i64);

struct Days(i64);

impl Years {
    pub fn to_days(&self) -> Days {
        Days(self.0 * 365)
    }
}


impl Days {
    /// 部分的な年を切り捨てます
    pub fn to_years(&self) -> Years {
        Years(self.0 / 365)
    }
}

fn old_enough(age: &Years) -> bool {
    age.0 >= 18
}

fn main() {
    let age = Years(5);
    let age_days = age.to_days();
    println!("Old enough {}", old_enough(&age));
    println!("Old enough {}", old_enough(&age_days.to_years()));
    // println!("Old enough {}", old_enough(&age_days));
}
```

最後のprint文をアンコメントしても、コンパイルが通りません。

基本型の値を`newtype`として作ると、このようにタプルのような構文で値が参照できます。
```rust, editable
struct Years(i64);

fn main() {
    let years = Years(42);
    let years_as_primitive: i64 = years.0;
}
```

### こちらも参照:

- [`structs`][struct]

[struct]: ../custom_types/structs.md

