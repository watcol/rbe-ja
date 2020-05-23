# ボックス、スタック、ヒープ

Rustではすべての変数がデフォルトでスタックに保存されます。しかし、`Box<T>`を
使うことで、値をヒープに保存することができます。`Box<T>`はヒープ上に保存された
型`T`の値をさすスマートポインタです。`Box`がスコープを出ると、デストラクタが
呼び出されることで、中の値も破棄され、ヒープメモリが開放されます。

`Box`内の値は`*`演算子で参照できます。これによって、間接参照のコストが
低くなります。

```rust,editable
use std::mem;

#[allow(dead_code)]
#[derive(Debug, Clone, Copy)]
struct Point {
    x: f64,
    y: f64,
}

// 長方形は左上の点と右下の点によって定義できます。
#[allow(dead_code)]
struct Rectangle {
    top_left: Point,
    bottom_right: Point,
}

fn origin() -> Point {
    Point { x: 0.0, y: 0.0 }
}

fn boxed_origin() -> Box<Point> {
    // ヒープ上に作られた原点を指すPointのポインタを返す
    Box::new(Point { x: 0.0, y: 0.0 })
}

fn main() {
    // (すべての型注釈は必要ないものです)
    // スタックに保存された変数
    let point: Point = origin();
    let rectangle: Rectangle = Rectangle {
        top_left: origin(),
        bottom_right: Point { x: 3.0, y: -4.0 }
    };

    // ヒープに保存された長方形
    let boxed_rectangle: Box<Rectangle> = Box::new(Rectangle {
        top_left: origin(),
        bottom_right: Point { x: 3.0, y: -4.0 },
    });

    // 関数の出力をBoxする
    let boxed_point: Box<Point> = Box::new(origin());

    // 二重参照
    let box_in_a_box: Box<Box<Point>> = Box::new(boxed_origin());

    println!("Point occupies {} bytes on the stack",  // Pointはスタック上の{}バイトを占有しています
             mem::size_of_val(&point));
    println!("Rectangle occupies {} bytes on the stack",  // Rectangleはスタック上の{}バイトを占有しています
             mem::size_of_val(&rectangle));

    // Boxのサイズはポインタのサイズと等しい
    println!("Boxed point occupies {} bytes on the stack",  // BoxされたPointはスタック上の{}バイトを占有しています
             mem::size_of_val(&boxed_point));
    println!("Boxed rectangle occupies {} bytes on the stack",  // BoxされたRectangleはスタック上の{}バイトを占有しています
             mem::size_of_val(&boxed_rectangle));
    println!("Boxed box occupies {} bytes on the stack",  // BoxされたBoxはスタック上の{}バイトを占有しています
             mem::size_of_val(&box_in_a_box));

    // `boxed_point`内のデータを`unboxed_point`にコピーする
    let unboxed_point: Point = *boxed_point;
    println!("Unboxed point occupies {} bytes on the stack",  // BoxされていないPointはスタック上の{}バイトを占有しています
             mem::size_of_val(&unboxed_point));
}
```
