# 幽霊型パラメータ

幽霊型は実行時には存在しませんが、コンパイル時に静的に
型チェックをするような型です。

データ型はマーカーとして余分なジェネリック型パラメータを持ち、それを使って
コンパイル時に型チェックすることができます。このパラメータはストレージの
余分なスペースを消費せず、実行時の動作にも影響しません。

以下の例では、幽霊型([std::marker::PhantomData])というマーカーを使って
それぞれ異なった型の値を持つタプルを作成します。

```rust,editable
use std::marker::PhantomData;

// ジェネリックな幽霊型タプル構造体。`A`と幽霊型`B`からなる。
#[derive(PartialEq)] // この型で`==`での比較を可能にする。
struct PhantomTuple<A, B>(A,PhantomData<B>);

// 同様に構造体を定義
#[derive(PartialEq)] // Allow equality test for this type.
struct PhantomStruct<A, B> { first: A, phantom: PhantomData<B> }

// 注意: ストレージはジェネリック型`A`のために確保されますが、`B`に対してはされません。
//       なので、`B`を処理に使うことはできません。

fn main() {
    // ここでは、`f32`と`f64`は幽霊型パラメータです。
    // PhantomTupleを`<char, f32>`で定義する。
    let _tuple1: PhantomTuple<char, f32> = PhantomTuple('Q', PhantomData);
    // PhantomTupleを`<char, f64>`で定義する。
    let _tuple2: PhantomTuple<char, f64> = PhantomTuple('Q', PhantomData);

    // `<char, f32>`で定義する
    let _struct1: PhantomStruct<char, f32> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };
    // `<char, f64>`で定義する。
    let _struct2: PhantomStruct<char, f64> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };
    
    // コンパイルエラー! 型が違うため比較できません
    //println!("_tuple1 == _tuple2 yields: {}",
    //          _tuple1 == _tuple2);
    
    // コンパイルエラー! 型が違うため比較できません
    //println!("_struct1 == _struct2 yields: {}",
    //          _struct1 == _struct2);
}
```

### こちらも参照:

- [継承][Derive]
- [構造体][struct]
- [タプル構造体][TupleStructs]

[Derive]: ../trait/derive.md
[struct]: ../custom_types/structs.md
[TupleStructs]: ../custom_types/structs.md
[std::marker::PhantomData]: https://doc.rust-lang.org/std/marker/struct.PhantomData.html
