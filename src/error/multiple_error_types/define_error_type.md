# エラー型を定義する

すべてのエラー型を一つの型で包んだ方がシンプルな場合もあります。
今回は、これを独自のエラーでやってみることにします。

Rustでは独自のエラー型を作成することができます。 一般的に、「良い」エラー型は:

* 異なるエラーを同じ型で表す
* わかりやすいエラーメッセージを提供する
* 他の型と比較しやすい
    - 良い例: `Err(EmptyVec)`
    - 悪い例: `Err("Please use a vector with at least one element".to_owned())`
* エラーについての情報を保持する
    - 良い例: `Err(BadChar(c, position))`
    - 悪い例: `Err("+ cannot be used here".to_owned())`
* 他のエラーと連携が取れる

```rust,editable
use std::fmt;

type Result<T> = std::result::Result<T, DoubleError>;

// 独自のエラー型を定義する。これはエラー処理の方法によってカスタマイズできる。
// ここで独自のエラーを起こしたり、下層のエラー実装を持ち越したり、その間で
// なにか処理をしたりすることができる。
#[derive(Debug, Clone)]
struct DoubleError;

// エラーの生成とどのようにそれが出力されるかということとは関係ありません。
// なので、出力のスタイルに複雑で乱雑なロジックを書く必要はありません。
//
// ここにはエラーに関するどんなデータも含まれていないことに注意してください。これは、
// どの処理が失敗したのかを知ることはできないということを意味します。
impl fmt::Display for DoubleError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "invalid first item to double")
    }
}

fn double_first(vec: Vec<&str>) -> Result<i32> {
    vec.first()
        // エラーを独自の新しい型に変換します。
        .ok_or(DoubleError)
        .and_then(|s| {
            s.parse::<i32>()
                // ここも新しい型に変換します。
                .map_err(|_| DoubleError)
                .map(|i| 2 * i)
        })
}

fn print(result: Result<i32>) {
    match result {
        Ok(n) => println!("The first doubled is {}", n),
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
