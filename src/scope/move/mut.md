# 可変性

所有権がムーブしたときに、データの可変性も変更できる。

```rust,editable
fn main() {
    let immutable_box = Box::new(5u32);

    println!("immutable_box contains {}", immutable_box);

    // 可変性のエラー
    //*immutable_box = 4;

    // 所有権(と可変性)を変更してBoxを*ムーブ*する。
    let mut mutable_box = immutable_box;

    println!("mutable_box contains {}", mutable_box);

    // Boxの要素を変更する
    *mutable_box = 4;

    println!("mutable_box now contains {}", mutable_box);
}
```
