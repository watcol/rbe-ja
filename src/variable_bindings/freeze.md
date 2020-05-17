# フリーズ

データが同じ名前の不変な変数に覆い隠された時、これも*フリーズ*します。
*フリーズされた*データは、不変な束縛がスコープを外れるまで、変更できません。

```rust,editable,ignore,mdbook-runnable
fn main() {
    let mut _mutable_integer = 7i32;

    {
        // 不変な`_mutable_integer`で覆い隠す
        let _mutable_integer = _mutable_integer;

        // エラー! `_mutable_integer`はこのスコープではフリーズされています。
        _mutable_integer = 50;
        // FIXME ^ この行をコメントアウトする

        // `_mutable_integer`がスコープを出る
    }

    // Ok! このスコープでは`_mutable_integer`はフリーズされていません。
    _mutable_integer = 3;
}
```
