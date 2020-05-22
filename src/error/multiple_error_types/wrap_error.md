# エラーをラップする

複数のエラーをボックス化するもう一つの方法は、独自の型でエラーを
ラップすることです。

```rust,editable
use std::error;
use std::num::ParseIntError;
use std::fmt;

type Result<T> = std::result::Result<T, DoubleError>;

#[derive(Debug)]
enum DoubleError {
    EmptyVec,
    // 変換エラーに関する実装を持ち越します。
    // さらなる情報を得るため、型にデータを追加します。
    Parse(ParseIntError),
}

impl fmt::Display for DoubleError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match *self {
            DoubleError::EmptyVec =>
                write!(f, "please use a vector with at least one element"),
            // これはラッパーであるため、元の型の関数を呼び出します。
            DoubleError::Parse(ref e) => e.fmt(f),
        }
    }
}

impl error::Error for DoubleError {
    fn source(&self) -> Option<&(dyn error::Error + 'static)> {
        match *self {
            DoubleError::EmptyVec => None,
            // 下層の実装のエラー型を返します。トレイとオブジェクトは暗示的に
            // `&error::Error`にキャストされます。下層の実装はすでに`Error`
            // トレイトを実装しているため、これは動作します。
            DoubleError::Parse(ref e) => Some(e),
        }
    }
}

// `ParseIntError`から`DoubleError`への変換を実装する。
// `?`から呼び出されたときに`ParseIntError`を
// `DoubleError`に変換するのに使われる。
impl From<ParseIntError> for DoubleError {
    fn from(err: ParseIntError) -> DoubleError {
        DoubleError::Parse(err)
    }
}

fn double_first(vec: Vec<&str>) -> Result<i32> {
    let first = vec.first().ok_or(DoubleError::EmptyVec)?;
    let parsed = first.parse::<i32>()?;

    Ok(2 * parsed)
}

fn print(result: Result<i32>) {
    match result {
        Ok(n)  => println!("The first doubled is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    print(double_first(numbers));
    print(double_first(empty));
    print(double_first(strings));
}
```

これは少し典型コードが多くなる上、すべてのアプリケーションに必要となるわけではありません。
個の典型コードを少なくするためのライブラリがいくつかあります。

### こちらも参照:

- [`From::from`][from]
- [`Enums`][enums]

[from]: https://doc.rust-lang.org/std/convert/trait.From.html
[enums]: ../../custom_types/enum.md
