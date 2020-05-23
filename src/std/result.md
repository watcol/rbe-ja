# `Result`

`Option`enumで失敗する可能性のある関数から値を返すことができました。
その場合、失敗したときは`None`を返します。しかし、しばしば*なぜ*関数が
失敗したのかが重要になってくることがあります。これは`Result`enumで表現します。

`Result<T, E>`enumは2つの列挙子を持ちます。

* `Ok(value)`は成功時に返され、`T`型の値`value`には結果が入ります。
* `Err(why)`は失敗時に返され、`E`型の値`why`には失敗の原因が入ります。

```rust,editable,ignore,mdbook-runnable
mod checked {
    // 数学的な「エラー」をキャッチする
    #[derive(Debug)]
    pub enum MathError {
        DivisionByZero,
        NonPositiveLogarithm,
        NegativeSquareRoot,
    }

    pub type MathResult = Result<f64, MathError>;

    pub fn div(x: f64, y: f64) -> MathResult {
        if y == 0.0 {
            // この操作は失敗なので、失敗の原因を`Err`
            // に入れて返します。
            Err(MathError::DivisionByZero)
        } else {
            // この操作は成功なので、結果を`Ok`に入れて返します。
            Ok(x / y)
        }
    }

    pub fn sqrt(x: f64) -> MathResult {
        if x < 0.0 {
            Err(MathError::NegativeSquareRoot)
        } else {
            Ok(x.sqrt())
        }
    }

    pub fn ln(x: f64) -> MathResult {
        if x <= 0.0 {
            Err(MathError::NonPositiveLogarithm)
        } else {
            Ok(x.ln())
        }
    }
}

// `op(x, y)`と`sqrt(ln(x / y))`は等価です
fn op(x: f64, y: f64) -> f64 {
    // 3段階matchピラミッドです!
    match checked::div(x, y) {
        Err(why) => panic!("{:?}", why),
        Ok(ratio) => match checked::ln(ratio) {
            Err(why) => panic!("{:?}", why),
            Ok(ln) => match checked::sqrt(ln) {
                Err(why) => panic!("{:?}", why),
                Ok(sqrt) => sqrt,
            },
        },
    }
}

fn main() {
    // 失敗しますか?
    println!("{}", op(1.0, 10.0));
}
```
