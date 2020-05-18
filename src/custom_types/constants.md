# 定数

Rustには2つの異なるタイプの定数があり、グローバルスコープを含むすべての場所で
宣言できます。どちらも明示的な型注釈が必要です。

* `const`: 変更できない値(普通はこっち)。
* `static`: `mut`にすることができる[`'static`][static]ライフタイムを持つ変数。
  'staticライフタイムであることは推論されるので、明示的に指定しなくても良い。
  可変なstatic変数にアクセス、変更することは[`unsafe`][unsafe]です。

```rust,editable,ignore,mdbook-runnable
// グローバル変数はあらゆるスコープの外で定義します。
static LANGUAGE: &str = "Rust";
const THRESHOLD: i32 = 10;  // THRESHOLD: しきい値

fn is_big(n: i32) -> bool {
    // 関数内から定数を参照
    n > THRESHOLD
}

fn main() {
    let n = 16;

    // main関数で定数にアクセス
    println!("This is {}", LANGUAGE);
    println!("The threshold is {}", THRESHOLD);
    println!("{} is {}", n, if is_big(n) { "big" } else { "small" });

    // エラー: `const`は変更できません。
    THRESHOLD = 5;
    // FIXME ^ この行をコメントアウトしてください
}
```

### こちらも参照:

- [The `const`/`static` RFC](https://github.com/rust-lang/rfcs/blob/master/text/0246-const-vs-static.md)
- [`'static`ライフタイム][static]

[static]: ../scope/lifetime/static_lifetime.md
[unsafe]: ../unsafe.md
