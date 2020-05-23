# ベクター

ベクターは可変長の配列です。スライスのように、コンパイル時にはサイズがわかりませんが、
自由にサイズを伸縮できます。ベクターは次の3要素からなります。
- データへのポインタ
- 長さ
- 容量

容量は、ベクターにどれだけのメモリが割り当てられているかを表します。容量を超えない
範囲で、ベクターの長さを伸ばすことができます。これを超える長さが必要になった時、
ベクターはもっと多い容量の場所へ再確保されます。 

```rust,editable,ignore,mdbook-runnable
fn main() {
    // イテレータはベクターに変換することができます。
    let collected_iterator: Vec<i32> = (0..10).collect();
    println!("Collected (0..10) into: {:?}", collected_iterator);

    // `vec!`マクロでベクターを初期化できます。
    let mut xs = vec![1i32, 2, 3];
    println!("Initial vector: {:?}", xs);

    // ベクターに新しい要素を入れる
    println!("Push 4 into the vector");
    xs.push(4);
    println!("Vector: {:?}", xs);

    // エラー! 不変なベクターは伸ばすことができません。
    collected_iterator.push(0);
    // FIXME ^ この行をコメントアウトしてください

    // `len`メソッドで現在のベクターの要素数を取得できます。
    println!("Vector length: {}", xs.len());

    // 角括弧で要素にアクセスできます。(インデックスは0から始まります)
    println!("Second element: {}", xs[1]);

    // `pop`はベクターから最後の要素を消し、それを返します。
    println!("Pop last element: {:?}", xs.pop());

    // 長さ以上のインデックスを指定するとパニックします。
    println!("Fourth element: {}", xs[3]);
    // FIXME ^ この行をコメントアウトしてください

    // `Vector`は簡単にイテレーションできます。
    println!("Contents of xs:");
    for x in xs.iter() {
        println!("> {}", x);
    }

    // enumerateを使って要素のインデックスを変数(`i`)に
    // とってイテレーションできます。
    for (i, x) in xs.iter().enumerate() {
        println!("In position {} we have value {}", i, x);
    }

    // `iter_mut`のおかげで、可変な`Vector`を変更しながら
    // イテレーションできます。
    for x in xs.iter_mut() {
        *x *= 3;
    }
    println!("Updated vector: {:?}", xs);
}
```

`Vec`のメソッドをさらに知りたいときは[std::vec][vec]モジュール
のドキュメントを参照してください。

[vec]: https://doc.rust-lang.org/std/vec/
