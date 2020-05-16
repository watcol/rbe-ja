# タプル

タプルは異なる型の値の集合です。タプルは`()`を使って表し、タプル自身の型は
`T1`、`T2`をメンバーの型だとして`(T1, T2, ...)`のように表します。タプルに
大きさの制限がないので、関数で複数の値を返すのにタプルが使われています。

```rust,editable
// タプルは、関数の引数や、返り値としても使えます。
fn reverse(pair: (i32, bool)) -> (bool, i32) {
    // `let` can be used to bind the members of a tuple to variables
    let (integer, boolean) = pair;

    (boolean, integer)
}

// これは演習のための構造体です。
#[derive(Debug)]
struct Matrix(f32, f32, f32, f32);

fn main() {
    // 複数の異なる型の値を束ねたタプル
    let long_tuple = (1u8, 2u16, 3u32, 4u64,
                      -1i8, -2i16, -3i32, -4i64,
                      0.1f32, 0.2f64,
                      'a', true);

    // タプルのインデックスを利用して、値を取り出すことができます。
    println!("long tuple first value: {}", long_tuple.0);
    println!("long tuple second value: {}", long_tuple.1);

    // タプルはタプルのメンバーになれます。
    let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);

    // タプルは出力できます。
    println!("tuple of tuples: {:?}", tuple_of_tuples);
    
    // ただし、長いタプルは出力できません。
    // let too_long_tuple = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13);
    // println!("too long tuple: {:?}", too_long_tuple);
    // TODO ^ コンパイルエラーを確認するために上の2行をアンコメントしてください。

    let pair = (1, true);
    println!("pair is {:?}", pair);

    println!("the reversed pair is {:?}", reverse(pair));

    // 一つしか要素がないタプルは、リテラルを括弧で囲ったものと
    // 区別するため、コンマが必要です。
    println!("one element tuple: {:?}", (5u32,)); // 要素1つのタプル: {:?}
    println!("just an integer: {:?}", (5u32)); // ただの整数: {:?}

    //タプルを分解してそれぞれを別の変数に束縛する。
    let tuple = (1, "hello", 4.5, true);

    let (a, b, c, d) = tuple;
    println!("{:?}, {:?}, {:?}, {:?}", a, b, c, d);

    let matrix = Matrix(1.1, 1.2, 2.1, 2.2);
    println!("{:?}", matrix);

}
```

### 演習

 1. *復習*: `fmt::Display`トレイトを上の例のMatrix構造体に実装し、
    デバッグフォーマット`{:?}`をディスプレイフォーマット`{}`に変えたときに
    以下のように表示されるようにしてください。

    ```text
    ( 1.1 1.2 )
    ( 2.1 2.2 )
    ```

    [ディスプレイ][print_display]の例に戻る必要があるかもしれません。
 2. `reverse`関数をテンプレートとして、下のように、引数として
    Matrixを受け取り、2つの要素を交換したMatrixを返す関数`transpose`を追加
    してください。

    ```rust,ignore
    println!("Matrix:\n{}", matrix);
    println!("Transpose:\n{}", transpose(matrix));
    ```

    出力結果:

    ```text
    Matrix:
    ( 1.1 1.2 )
    ( 2.1 2.2 )
    Transpose:
    ( 1.1 2.1 )
    ( 1.2 2.2 )
    ```

[print_display]: ../hello/print/print_display.md
