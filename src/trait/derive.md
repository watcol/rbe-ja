# 継承

コンパイラは、`#[derive]`[属性][attribute]によって、いくつかのトレイトの
標準実装を提供しています。これらのトレイトは、もっと複雑な振る舞いが必要な
場合、手動で実装することもできます。

以下が継承できるトレイトの一覧です。
* 比較トレイト:
  [`Eq`][eq]、[`PartialEq`][partial-eq]、[`Ord`][ord]、[`PartialOrd`][partial-ord]。
* `&T`をコピーして`T`を作成する[`Clone`][clone]
* 'ムーブを行わず、コピーするための[`Copy`][copy]
* `&T`からハッシュを算出する[`Hash`][hash]
* データ型のからのインスタンスを作る[`Default`][default]
* `{:?}`フォーマットで値を出力する[`Debug`][debug]
 
```rust,editable
// `Centimeters`は比較できるタプル構造体です。
#[derive(PartialEq, PartialOrd)]
struct Centimeters(f64);

// `Inches`は出力できるタプル構造体です。
#[derive(Debug)]
struct Inches(i32);

impl Inches {
    fn to_centimeters(&self) -> Centimeters {
        let &Inches(inches) = self;

        Centimeters(inches as f64 * 2.54)
    }
}

// `Seconds`は何も属性がないタプル構造体です。
struct Seconds(i32);

fn main() {
    let _one_second = Seconds(1);

    // エラー: `Seconds`は`Debug`トレイトを実装していないためプリントできません。
    //println!("One second looks like: {:?}", _one_second);
    // TODO ^ この行をアンコメントしてみてください

    // エラー: `Seconds`は`PartialEq`トレイトを実装していないため比較できません。
    //let _this_is_true = (_one_second == _one_second);
    // TODO ^ この行をアンコメントしてみてください

    let foot = Inches(12);

    println!("One foot equals {:?}", foot);

    let meter = Centimeters(100.0);

    let cmp =
        if foot.to_centimeters() < meter {
            "smaller"
        } else {
            "bigger"
        };

    println!("One foot is {} than one meter.", cmp);
}
```

### こちらも参照:
- [`derive`][derive]

[attribute]: ../attribute.md
[eq]: https://doc.rust-lang.org/std/cmp/trait.Eq.html
[partial-eq]: https://doc.rust-lang.org/std/cmp/trait.PartialEq.html
[ord]: https://doc.rust-lang.org/std/cmp/trait.Ord.html
[partial-ord]: https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html
[clone]: https://doc.rust-lang.org/std/clone/trait.Clone.html
[copy]: https://doc.rust-lang.org/core/marker/trait.Copy.html
[hash]: https://doc.rust-lang.org/std/hash/trait.Hash.html
[default]: https://doc.rust-lang.org/std/default/trait.Default.html
[debug]: https://doc.rust-lang.org/std/fmt/trait.Debug.html
[derive]: https://doc.rust-lang.org/reference/attributes.html#derive
