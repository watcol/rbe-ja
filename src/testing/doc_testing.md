# ドキュメンテーションテスト

Rustプロジェクトのドキュメントを作る簡単な方法は、ソースに注釈を付けることです。
コメントドキュメントは[markdown]で書くことができ、コードブロックをサポートしています。
また、コードブロックをテストとして扱うことができます。

```rust,ignore
/// 最初の行は関数の簡単な説明です。
///
/// 次の行から詳しい説明が始まります。コードブロックは
/// 3つのバッククオートや`fn main()`、`extern crate <cratename>`
/// などで始まり、`doccomments`クレートで実行されることが想定されています。
///
/// ```
/// let result = doccomments::add(2, 3);
/// assert_eq!(result, 5);
/// ```
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

/// 普通ドキュメンテーションコメントは"Examples"(例)、"Panics"(パニック)、"Failures"(失敗)
/// のような章を持ちます
///
/// 次の関数で2つの関数を割ります。
///
/// # Examples
///
/// ```
/// let result = doccomments::div(10, 2);
/// assert_eq!(result, 5);
/// ```
///
/// # Panics
///
/// この関数は第2引数が0のときにパニックします。
///
/// ```rust,should_panic
/// // panics on division by zero
/// doccomments::div(10, 0);
/// ```
pub fn div(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("Divide-by-zero error");
    }

    a / b
}
```

テストは`cargo test`で実行できます。

```shell
$ cargo test
running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests doccomments

running 3 tests
test src/lib.rs - add (line 7) ... ok
test src/lib.rs - div (line 21) ... ok
test src/lib.rs - div (line 31) ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## ドキュメンテーションテストのモチベーション

ドキュメンテーションテストの主な目的は機能を練習するための例を提供することであり、
これは重要な[ガイドライン][question-instead-of-unwrap]です。これによりドキュメント
のコード片が使用できるようになります。しかし、`main`関数は`unit`を返すため、
`?`を使うとコンパイルエラーが起きます。ソースの行を隠すことでこれを防げます。
例えば、`fn try_main() -> Result<(), ErrorType>`を書いたとしても、これを隠し、
さらに`main`内の`unwrap`も隠すことができます。複雑に聞こえますか? これが例です。

```rust,ignore
/// ドキュメンテーションテストで使う`try_main`を隠す
///
/// ```
/// # // `#`で行を隠すことができますが、これはまだコンパイルできます!
/// # fn try_main() -> Result<(), String> { // ドキュメントで示す行をラップする
/// let res = try::try_div(10, 2)?;
/// # Ok(()) // try_mainから値を返す
/// # }
/// # fn main() { // unwrap()するmain関数を書く。
/// #    try_main().unwrap(); // try_mainを呼び、unwrapする
/// #                         // これでテストが失敗したときにパニックする
/// # }
/// ```
pub fn try_div(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err(String::from("Divide-by-zero"))
    } else {
        Ok(a / b)
    }
}
```

## こちらも参照

* [RFC505][RFC505]のドキュメンテーションスタイル
* [API Guidelines][doc-nursery]のドキュメンテーションガイドライン

[doc-nursery]: https://rust-lang-nursery.github.io/api-guidelines/documentation.html
[markdown]: https://daringfireball.net/projects/markdown/
[RFC505]: https://github.com/rust-lang/rfcs/blob/master/text/0505-api-comment-conventions.md
[question-instead-of-unwrap]: https://rust-lang-nursery.github.io/api-guidelines/documentation.html#examples-use--not-try-not-unwrap-c-question-mark
