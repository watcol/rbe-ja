# エイリアシング

データは何度でも不変借用できますが、不変借用されている間はもとのデータは可変借用
できません。言い換えれば、同時に*一つ*の可変参照ができ、もとのデータは可変参照が
最後に使われた*後*でのみ借用できるということです。

```rust,editable
struct Point { x: i32, y: i32, z: i32 }

fn main() {
    let mut point = Point { x: 0, y: 0, z: 0 };

    let borrowed_point = &point;
    let another_borrow = &point;

    // データには参照または所有者のみアクセスできます。
    println!("Point has coordinates: ({}, {}, {})",
                borrowed_point.x, another_borrow.y, point.z);

    // エラー! 現在不変参照されているため、`point`を可変参照
    // することはできません。
    // let mutable_borrow = &mut point;
    // TODO ^ この行をアンコメントしてみてください

    // 借用された値はもう一度使用できます。
    println!("Point has coordinates: ({}, {}, {})",
                borrowed_point.x, another_borrow.y, point.z);

    // もう不変参照は使われないため、残りのコードでは
    // 可変参照として再び借用できます。
    let mutable_borrow = &mut point;

    // 可変参照でデータを変更する
    mutable_borrow.x = 5;
    mutable_borrow.y = 2;
    mutable_borrow.z = 1;

    // エラー! `point`は現在可変借用されているため、不変借用はできません。
    // let y = &point.y;
    // TODO ^ この行をアンコメントしてみてください

    // エラー! `println!`は不変参照を必要とするため、呼び出せません。
    // println!("Point Z coordinate is {}", point.z);
    // TODO ^ この行をアンコメントしてみてください

    // Ok! 可変参照は不変参照として`println!`に渡せます
    println!("Point has coordinates: ({}, {}, {})",
                mutable_borrow.x, mutable_borrow.y, mutable_borrow.z);

    // 可変参照はもう使われないので、残りのコードでは再度借用できます。
    let new_borrowed_point = &point;
    println!("Point now has coordinates: ({}, {}, {})",
             new_borrowed_point.x, new_borrowed_point.y, new_borrowed_point.z);
}
```
