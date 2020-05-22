# `?`の他の使い方

前の例では、`parse`を即座に`map`に渡して、エラーを`Box`に格納していました。

```rust,ignore
.and_then(|s| s.parse::<i32>()
    .map_err(|e| e.into())
```

これは一般的な操作なので、利便性のため省略することができます。
悲しいかな、`and_then`は柔軟性が低いため、できません。しかし、
`?`を使えばできます。

`?`は`unwrap`や`return Err(err)`と同じだと説明しました。これは大体合って
いますが、実際には`unwrap`や`return Err(From::from(err))`です。
`From::from`は他の型に変更するツールであるため、これは`?`によって返り値の型
に変換できることを意味しています。

ここで、前の例を`?`を使って書き直してみましょう。結果的に、`map_err`は
`From::from`がエラー型に実装されていることから不要になります。

```rust,editable
use std::error;
use std::fmt;

// エイリアスを`Box<dyn error::Error>`に変更します。
type Result<T> = std::result::Result<T, Box<dyn error::Error>>;

#[derive(Debug)]
struct EmptyVec;

impl fmt::Display for EmptyVec {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "invalid first item to double")
    }
}

impl error::Error for EmptyVec {}

// 前と同じ構造ですが、すべての`Results`と`Options`を一つにつながず、
// `?`から値を変数に代入しています。
fn double_first(vec: Vec<&str>) -> Result<i32> {
    let first = vec.first().ok_or(EmptyVec)?;
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

これで非常にきれいになりました。もともとの`panic`と比べると、返り値の型が
`Result`担ったことを除いて、`unwrap`が`?`に変わっただけであることがわかります。
また、`Result`はトップレベルで分解する必要があります。

### こちらも参照:

- [`From::from`][from]
- [`?`][q_mark]

[from]: https://doc.rust-lang.org/std/convert/trait.From.html
[q_mark]: https://doc.rust-lang.org/reference/expressions/operator-expr.html#the-question-mark-operator
