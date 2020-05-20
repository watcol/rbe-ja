# テストケース: 単位の明確化

共通の単位同士を扱う際のチェックのために、`Add`を幽霊型を用いた実装に
すると便利です。その場合`Add`トレイトは以下のようになります。

```rust,ignore
// このように定義しておくと、`Self + RHS(右辺値) = Output`であることが保証され、
// 実装時にRHSを省略すると、デフォルトでSelfと同じになります。
pub trait Add<RHS = Self> {
    type Output;

    fn add(self, rhs: RHS) -> Self::Output;
}

// `T<U> + T<U> = T<U>`であるため、`Output`は`T<U>`である必要があります。
impl<U> Add for T<U> {
    type Output = T<U>;
    ...
}
```

以下は全体を示した例です。

```rust,editable
use std::ops::Add;
use std::marker::PhantomData;

/// ユニット型を定義する。
#[derive(Debug, Clone, Copy)]
enum Inch {}
#[derive(Debug, Clone, Copy)]
enum Mm {}

/// `Length`は`Unit`型の幽霊型パラメータを持つ型
/// そして長さの型はジェネリックではありません(`f64`です)。
///
/// `f64`ははじめから`Clone`と`Copy`トレイトを実装しています。
#[derive(Debug, Clone, Copy)]
struct Length<Unit>(f64, PhantomData<Unit>);

/// `Add`トレイトで`+`演算子の振る舞いを定義します。
impl<Unit> Add for Length<Unit> {
     type Output = Length<Unit>;

    // add()は合計値を含む新しい`Length`構造体を返します。
    fn add(self, rhs: Length<Unit>) -> Length<Unit> {
        // `+`は`f64`に対する`Add`の実装を呼び出します。
        Length(self.0 + rhs.0, PhantomData)
    }
}

fn main() {
    // `one_foot`は幽霊型パラメータ`Inch`を持つ
    let one_foot:  Length<Inch> = Length(12.0, PhantomData);
    // `one_meter`は幽霊型パラメータ`Mm`を持つ
    let one_meter: Length<Mm>   = Length(1000.0, PhantomData);

    // `+`は`Length<Unit>`の`add()`メソッドを呼び出します。
    //
    // `Length`は`Copy`を実装しているので、`add()`は
    // `one_foot`や`one_meter`を消費せず、コピーします。
    let two_feet = one_foot + one_foot;
    let two_meters = one_meter + one_meter;

    // 問題なく実行されていることの確認
    println!("one foot + one_foot = {:?} in", two_feet.0);
    println!("one meter + one_meter = {:?} mm", two_meters.0);

    // 違う単位間では計算できない。
    // コンパイルエラー: 型が違います。
    //let one_feter = one_foot + one_meter;
}
```

### こちらも参照:

- [借用(`&`)][Borrowing (`&`)]
- [境界(`X:Y`)][Bounds (`X: Y`)]
- [enum]
- [implとself][impl & self]
- [オーバーロード][Overloading]
- [参照][ref]
- [トレイト(`X for Y`)][Traits (`X for Y`)]
- [タプル構造体][TupleStructs]

[Borrowing (`&`)]: ../../scope/borrow.md
[Bounds (`X: Y`)]: ../../generics/bounds.md
[enum]: ../../custom_types/enum.md
[impl & self]: ../../fn/methods.md
[Overloading]: ../../trait/ops.md
[ref]: ../../scope/borrow/ref.md
[Traits (`X for Y`)]: ../../trait.md
[TupleStructs]: ../../custom_types/structs.md
[std::marker::PhantomData]: https://doc.rust-lang.org/std/marker/struct.PhantomData.html
