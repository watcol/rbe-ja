# メソッド

メソッドはオブジェクトに関連付けられた関数です。メソッドは`self`キーワードで
オブジェクトのデータや、他のメソッドにアクセスできます。メソッドは`impl`
ブロック内で定義します。

```rust,editable
struct Point {
    x: f64,
    y: f64,
}

// 実装ブロックです。`Point`のすべてのメソッドはここで定義されます。
impl Point {
    // これは静的メソッドです。
    // 静的メソッドは、インスタンスから呼び出す必要はなく、
    // 一般的にコンストラクタとして使われます。
    fn origin() -> Point {
        Point { x: 0.0, y: 0.0 }
    }

    // 2つの引数をとるもう一つの静的メソッドです。
    fn new(x: f64, y: f64) -> Point {
        Point { x: x, y: y }
    }
}

struct Rectangle {
    p1: Point,
    p2: Point,
}

impl Rectangle {
    // これはインスタンスメソッドです。
    // `&self`は`self: &Self`の糖衣構文であり、`Self`は呼び出し元の
    // オブジェクトの型です。ここでは`Self`は`Rectangle`と等価です。
    fn area(&self) -> f64 {
        // ドット演算子で`self`のフィールドにアクセスできます。
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        // `abs`は`f64`のメソッドで、呼び出し元の絶対値を返します。
        ((x1 - x2) * (y1 - y2)).abs()
    }

    fn perimeter(&self) -> f64 {
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        2.0 * ((x1 - x2).abs() + (y1 - y2).abs())
    }

    // このメソッドは呼び出し元を可変にする必要があります。
    // `&mut self`は`self: &mut Self`の糖衣構文です。
    fn translate(&mut self, x: f64, y: f64) {
        self.p1.x += x;
        self.p2.x += x;

        self.p1.y += y;
        self.p2.y += y;
    }
}

// `Pair`は2つのヒープ上に割り当てられた整数を持ちます。
struct Pair(Box<i32>, Box<i32>);

impl Pair {
    // このメソッドは呼び出し元オブジェクトのリソースを「消費」します。
    // `self`は`self: Self`の糖衣構文です。
    fn destroy(self) {
        // `self`を分割代入する。
        let Pair(first, second) = self;

        println!("Destroying Pair({}, {})", first, second);  // Pair({}, {})を破壊しました。

        // `first`と`second`はスコープを出て、開放されます。
    }
}

fn main() {
    let rectangle = Rectangle {
        // 静的メソッドはコロン2つで呼び出します。
        p1: Point::origin(),
        p2: Point::new(3.0, 4.0),
    };

    // インスタンスメソッドはドット演算子で呼び出します。つまり、
    // 第一引数`&self`は暗示的に渡されます。`rectangle.perimeter()`は
    // `Rectangle::perimeter(&rectangle)`と同等です。
    println!("Rectangle perimeter: {}", rectangle.perimeter());
    println!("Rectangle area: {}", rectangle.area());

    let mut square = Rectangle {
        p1: Point::origin(),
        p2: Point::new(1.0, 1.0),
    };

    // エラー! `rectangle`は不変ですが、メソッドはオブジェクトが可変である
    // ことを要求しています。
    //rectangle.translate(1.0, 0.0);
    // TODO ^ この行をアンコメントしてみてください。

    // OK! 可変な変数は可変なメソッドを呼び出せます。
    square.translate(1.0, 1.0);

    let pair = Pair(Box::new(1), Box::new(2));

    pair.destroy();

    // エラー! 前の`destroy`で`pair`を「消費」しました。
    //pair.destroy();
    // TODO ^ この行をアンコメントしてください。
}
```
