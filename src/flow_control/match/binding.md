# 束縛

変数を直接マッチさせないときは、分割代入なしで自分自身を参照
できません。`match`は自分自身を束縛する`@`記号を提供しています。

```rust,editable
// `age`関数は`u32`を返します
fn age() -> u32 {
    15
}

fn main() {
    println!("Tell me what type of person you are");  // あなたがどんな人かおしえて

    match age() {
        0             => println!("I'm not born yet I guess"),  // 多分私はまだ生まれていません。
        // 1 ..= 12にはマッチできますが、この子供の歳がわかりません。
        // 代わりに、1 ..= 12の数値を束縛する変数nを用意します。
        // これで歳がわかります。
        n @ 1  ..= 12 => println!("I'm a child of age {:?}", n),  // 私は{:?}歳の子供です
        n @ 13 ..= 19 => println!("I'm a teen of age {:?}", n),  // 私は{:?}歳のティーン(訳注: 10代の若者)です。
        // マッチしなかった場合の処理
        n             => println!("I'm an old person of age {:?}", n),
    }
}
```

`Option`のように、「分割代入」した`enum`の列挙子にも使えます。

```rust,editable
fn some_number() -> Option<u32> {
    Some(42)
}

fn main() {
    match some_number() {
        // `Some`列挙子で、値が42に等しかったときに、マッチして
        // `n`に束縛する。
        Some(n @ 42) => println!("The Answer: {}!", n),  // 答え: {}!
        // 他の数値にマッチする。
        Some(n)      => println!("Not interesting... {}", n),  // 面白くない... {}
        // 他のもの(`None`列挙子)にマッチする。
        _            => (),
    }
}
```

### こちらも参照:
- [`functions`][functions]
- [`enums`][enums]
- [`Option`][option]

[functions]: ../../fn.md
[enums]: ../../custom_types/enum.md
[option]: ../../std/option.md
