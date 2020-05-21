# refパターン

パターンマッチングや`let`束縛による分割代入をするとき、構造体やタプルのフィールド
の参照をとるのに`ref`キーワードが使用できます。下の例では、有用な使い方をいくつか
載せています。

```rust,editable
#[derive(Clone, Copy)]
struct Point { x: i32, y: i32 }

fn main() {
    let c = 'Q';

    // `ref`を使って左辺で借用を表している。
    // 右辺で`&`を使って借用するのと等価
    let ref ref_c1 = c;
    let ref_c2 = &c;

    println!("ref_c1 equals ref_c2: {}", *ref_c1 == *ref_c2);

    let point = Point { x: 0, y: 0 };

    // `ref`は構造体の分割代入にも使える。
    let _copy_of_x = {
        // `ref_to_x`は`point`のフィールド`x`の参照
        let Point { x: ref ref_to_x, y: _ } = point;

        // `point`の`x`フィールドのコピーを返す
        *ref_to_x
    };

    // `point`の可変コピー
    let mut mutable_point = point;

    {
        // `ref`と`mut`を一緒に使うと可変参照が取れる。
        let Point { x: _, y: ref mut mut_ref_to_y } = mutable_point;

        // `mutable_point`の`y`フィールドを可変参照から変更する。
        *mut_ref_to_y = 1;
    }

    println!("point is ({}, {})", point.x, point.y);
    println!("mutable_point is ({}, {})", mutable_point.x, mutable_point.y);

    // ポインタを含む可変なタプル
    let mut mutable_tuple = (Box::new(5u32), 3u32);
    
    {
        // `mutable_tuple`を分割代入し、`last`の値を変更する。
        let (_, ref mut last) = mutable_tuple;
        *last = 2u32;
    }
    
    println!("tuple is {:?}", mutable_tuple);
}
```
