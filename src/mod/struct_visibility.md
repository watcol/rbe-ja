# 構造体の可視性

構造体はフィールド内での可視性も持っています。デフォルトではプライベートで、
`pub`修飾子で上書きできます。この可視性は、定義されたモジュールの外からアクセスする
ときのみ有効で、情報を隠す(カプセル化する)ことを目的としています。

```rust,editable
mod my {
    // ジェネリック型`T`のパブリックフィールドを持ったパブリックな構造体
    pub struct OpenBox<T> {
        pub contents: T,
    }

    // ジェネリック型`T`のプライベートフィールドを持ったパブリックな構造体
    #[allow(dead_code)]
    pub struct ClosedBox<T> {
        contents: T,
    }

    impl<T> ClosedBox<T> {
        // パブリックなコンストラクタメソッド
        pub fn new(contents: T) -> ClosedBox<T> {
            ClosedBox {
                contents: contents,
            }
        }
    }
}

fn main() {
    // パブリックフィールドを持ったパブリックな構造体はいつものように宣言できます。
    let open_box = my::OpenBox { contents: "public information" };

    // そしてフィールドにも普通にアクセスできます。
    println!("The open box contains: {}", open_box.contents);

    // プライベートフィールドを持ったパブリックな構造体はフィールド名で宣言できません。
    // エラー! `ClosedBox`にはプライベートフィールドがあります。
    //let closed_box = my::ClosedBox { contents: "classified information" };
    // TODO ^ この行をアンコメントしてみてください

    // しかし、パブリックなコンストラクタを使って初期化できます。
    let _closed_box = my::ClosedBox::new("classified information");

    // そしてプライベートなフィールドにはアクセスできません
    // エラー! `contents`フィールドはプライベートです
    //println!("The closed box contains: {}", _closed_box.contents);
    // TODO ^ この行をアンコメントしてみてください
}
```

### こちらも参照:

- [ジェネリック][generics]
- [メソッド][methods]

[generics]: ../generics.md
[methods]: ../fn/methods.md
