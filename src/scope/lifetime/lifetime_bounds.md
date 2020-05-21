# ライフタイムの境界

ジェネリックに境界が設定できたように、ライフタイム(これもジェネリックです)
にも境界が使えます。`:`はここでは違った意味を持ちますが、`+`は同じ意味です。
以下の注意を読んでください。

1. `T: 'a`: `T`の*すべて*の参照が`'a`より長生きする必要がある。
2. `T: Trait + 'a`: 型`T`は`Trait`トレイトを実装している必要があり、`T`の
*すべて*の参照が`'a`より長生きする必要がある。

下の例では`where`節を使って上のことを表現しています。

```rust,editable
use std::fmt::Debug; // 境界として設定するトレイト

#[derive(Debug)]
struct Ref<'a, T: 'a>(&'a T);
// `Ref`はジェネリック型`T`への参照を持ち、それは未知のライフタイム`'a`
// を持ちます。`T`は`T`のすべての*参照*が`'a`より長生きする必要があり、
// `Ref`のライフタイムは`'a`を超えられません。

// `Debug`トレイトを持つジェネリック型`T`をプリントする関数
fn print<T>(t: T) where
    T: Debug {
    println!("`print`: t is {:?}", t);
}

// `Debug`を実装し、`T`すべての参照が`'a`より長生きするような
// ジェネリック型`T`の参照をとります。さらに、`'a`は関数より
// 長生きする必要があります。
fn print_ref<'a, T>(t: &'a T) where
    T: Debug + 'a {
    println!("`print_ref`: t is {:?}", t);
}

fn main() {
    let x = 7;
    let ref_x = Ref(&x);

    print_ref(&ref_x);
    print(ref_x);
}
```

### こちらも参照:

- [ジェネリック][generics]
- [ジェネリックの境界][bounds]
- [ジェネリックでの複数の境界][multibounds]

[generics]: ../../generics.md
[bounds]: ../../generics/bounds.md
[multibounds]: ../../generics/multi_bounds.md
