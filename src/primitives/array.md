# 配列とスライス

配列は、メモリ上に連続して保存される、同じ型`T`の値の集合です。配列は角括弧`[]`
で作ることができます。また、長さはコンパイル時にわかっていなければならず、
`[T; size]`で型の定義ができます。

Slicesは配列と似ていますが、コンパイル時に長さが決まっていなくても良いです。
その代わり、データへのポインタと、スライスの長さという2つのデータを格納しなければ
なりません。1つのデータのサイズはアーキテクチャ依存(例えばx86-64なら64ビット)で、
`usize`と同じ大きさです。
スライスは配列の一部分の借用として使うことができ、`&[T]`という型指定子が使えます。

```rust,editable,ignore,mdbook-runnable
use std::mem;

// この関数はスライスを借用します。
fn analyze_slice(slice: &[i32]) {
    println!("first element of the slice: {}", slice[0]);
    println!("the slice has {} elements", slice.len());
}

fn main() {
    // 固定長配列 (型シグネチャは余計です)
    let xs: [i32; 5] = [1, 2, 3, 4, 5];

    // すべての要素を同じ値で初期化することができます。
    let ys: [i32; 500] = [0; 500];

    // インデックスは0から始まります。
    println!("first element of the array: {}", xs[0]);  // 配列の最初の要素: {}
    println!("second element of the array: {}", xs[1]);  // 配列の2つ目の要素: {}

    // `len`は配列の長さを返します。
    println!("array size: {}", xs.len());

    // 配列はスタック上に置かれます。
    println!("array occupies {} bytes", mem::size_of_val(&xs));  // 配列は{}バイト占有しています。

    // 配列は自動的にスライスとして借用されます。
    println!("borrow the whole array as a slice");  // 配列の全体をスライスとして借用する
    analyze_slice(&xs);

    // スライスで配列の部分を指す
    // [スタート..エンド]のようにして指定します。
    // スタートはスライスの最初の位置のインデックス
    // エンドは1より大きいスライスの最後の位置のインデックスです。
    println!("borrow a section of the array as a slice");  // 配列の部分をスライスとして借用する。
    analyze_slice(&ys[1 .. 4]);

    // 範囲外の位置を指定するとコンパイルエラーが起こる。
    println!("{}", xs[5]);
}
```
