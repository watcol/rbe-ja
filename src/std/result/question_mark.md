# `?`

Resultの処理をチェーンすると、だらしないです。幸運なことに、`?`演算子を使えば
きれいに書くことができます。`Result`を返す`?`を式の最後につけると、match式のように、
`Err(err)`ならば`Err(From::from(err))`を返して関数を終了し、`Ok(ok)`ならば`ok`を返します。

```rust,editable,ignore,mdbook-runnable
mod checked {
    #[derive(Debug)]
    enum MathError {
        DivisionByZero,
        NonPositiveLogarithm,
        NegativeSquareRoot,
    }

    type MathResult = Result<f64, MathError>;

    fn div(x: f64, y: f64) -> MathResult {
        if y == 0.0 {
            Err(MathError::DivisionByZero)
        } else {
            Ok(x / y)
        }
    }

    fn sqrt(x: f64) -> MathResult {
        if x < 0.0 {
            Err(MathError::NegativeSquareRoot)
        } else {
            Ok(x.sqrt())
        }
    }

    fn ln(x: f64) -> MathResult {
        if x <= 0.0 {
            Err(MathError::NonPositiveLogarithm)
        } else {
            Ok(x.ln())
        }
    }

    // 中間関数
    fn op_(x: f64, y: f64) -> MathResult {
        // `div`が失敗したら、`DivisionByZero`が`return`される
        let ratio = div(x, y)?;

        // `ln`が失敗したら、`NonPositiveLogarithm`が`return`される
        let ln = ln(ratio)?;

        sqrt(ln)
    }

    pub fn op(x: f64, y: f64) {
        match op_(x, y) {
            Err(why) => panic!(match why {
                MathError::NonPositiveLogarithm
                    => "logarithm of non-positive number",
                MathError::DivisionByZero
                    => "division by zero",
                MathError::NegativeSquareRoot
                    => "square root of negative number",
            }),
            Ok(value) => println!("{}", value),
        }
    }
}

fn main() {
    checked::op(1.0, 10.0);
}
```

[ドキュメンテーション][docs]を参照し、`Result`を
操作するたくさんのメソッドを探してみてください。

[docs]: https://doc.rust-lang.org/std/result/index.html
