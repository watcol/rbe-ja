# 属性

属性は、モジュール、クレート、またはその要素などに対して適用されるメタデータです。
このメタデータは次のような用途に使われます。

<!-- TODO: Link these to their respective examples -->

* [条件に合致するときのみコードをコンパイルする][cfg]
* [クレート名、バージョン、タイプ(バイナリかライブラリ)を指定する][crate]
* [リント][lint](警告)を無効化する
* コンパイラの機能を有効化する(マクロ、グロブ、インポートなど)
* 外部ライブラリにリンクする
* 関数をユニットテストとしてマークする
* 関数をベンチマークとしてマークする

クレート全体に属性を適用するときは、`#![crate_attribute]`構文を使い、
モジュールや要素に対して適用するときは、`#[item_attribute]`構文を使います
(`!`を忘れないでください)。

属性はこのような構文で引数を取ることができます:

* `#[attribute = "value"]`
* `#[attribute(key = "value")]`
* `#[attribute(value)]`

属性はこのようにして複数の引数を取ることもできます:

```rust,ignore
#[attribute(value, value2)]


#[attribute(value, value2, value3,
            value4, value5)]
```

[cfg]: attribute/cfg.md
[crate]: attribute/crate.md
[lint]: https://en.wikipedia.org/wiki/Lint_%28software%29
