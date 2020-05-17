# 構造体

`struct`キーワードで作ることができる構造体には3つのタイプがあります。

* タプル構造体。基本的な名前がついたタプル。
* 古典的な[Cの構造体][c_struct]
* ユニット構造体。フィールドを持たず、ジェネリックを扱うときに有用です。

```rust,editable
#[derive(Debug)]
struct Person<'a> {
    // 'aでライフタイムを定義しています。
    name: &'a str,
    age: u8,
}

// ユニット構造体
struct Unit;

// タプル構造体
struct Pair(i32, f32);

// 2個のフィールドを持つ構造体
struct Point {
    x: f32,
    y: f32,
}

// 構造体は構造体のフィールドとして再利用できます。
#[allow(dead_code)]
struct Rectangle {
    // 長方形は左上と右下の位置で定義できます。
    // corners are in space.
    top_left: Point,
    bottom_right: Point,
}

fn main() {
    // 構造体を作って簡潔に初期化する。
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age };

    // 構造体のデバッグプリント
    println!("{:?}", peter);


    // `Point`をインスタンス化する
    let point: Point = Point { x: 10.3, y: 0.4 };

    // Pointのフィールドにアクセスする
    println!("point coordinates: ({}, {})", point.x, point.y);

    // アップデート構文で他の構造体のフィールドを再利用して新しい構造体を作る。
    let bottom_right = Point { x: 5.2, ..point };

    // `point`からフィールドを再利用したため、`bottom_right.y`は`point.y`と
    // 同じになります。
    println!("second point: ({}, {})", bottom_right.x, bottom_right.y);

    // `let`でpointを分割代入して束縛する
    let Point { x: top_edge, y: left_edge } = point;

    let _rectangle = Rectangle {
        // 構造体のインスタンス化も式です。
        top_left: Point { x: left_edge, y: top_edge },
        bottom_right: bottom_right,
    };

    // ユニット構造体のインスタンス化
    let _unit = Unit;

    // タプル構造体のインスタンス化
    let pair = Pair(1, 0.1);

    // タプル構造体のフィールドにアクセスする
    println!("pair contains {:?} and {:?}", pair.0, pair.1);

    // タプル構造体を分割代入
    let Pair(integer, decimal) = pair;

    println!("pair contains {:?} and {:?}", integer, decimal);
}
```

### 演習

1. 長方形の面積を計算する`rect_area`関数を追加してください。(ネスト分割代入を試してみてください。)
2. `Point`と`f32`を引数にとり、`Point`を左上の点、`f32`を一辺の長さとして正方形を作成して`Rectangle`として返す関数`square`を追加してください。

### こちらも参照:

[`attributes`][attributes]、[ライフタイム][lifetime]、そして[分割代入][destructuring]

[attributes]: ../attribute.md
[c_struct]: https://en.wikipedia.org/wiki/Struct_(C_programming_language)
[destructuring]: ../flow_control/match/destructuring.md
[lifetime]: ../scope/lifetime.md
