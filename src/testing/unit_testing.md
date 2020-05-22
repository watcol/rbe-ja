# ユニットテスト

テストはコードが期待した動作をすることを確かめるためのRustの機能です。テスト関数は
典型的にはいくつかのセットアップをして、テストしたいコードを書き、期待したものと
合致しているか確かめます。

ユニットテストの多くは`#[cfg(test)]`[属性][attribute]のついたtests[モジュール][mod]
に入っていて、テスト関数には`#[test]`属性がついています。

テストは、関数が[panic][panic]したときに失敗し、そのための[マクロ][macros]がいくつかあります。

* `assert!(expression)` - 式が`false`ならパニックする
* `assert_eq!(left, right)`と`assert_ne!(left, right)` - 右と左の式が
  それぞれ違うとき、同じときにパニックする

```rust,ignore
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

// これは失敗する例なので、間違った加算関数になっています。
#[allow(dead_code)]
fn bad_add(a: i32, b: i32) -> i32 {
    a - b
}

#[cfg(test)]
mod tests {
    // これでモジュールの外側の関数をすべてスコープに導入できます。
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(1, 2), 3);
    }

    #[test]
    fn test_bad_add() {
        // この確認は失敗します。
        // プライベート関数もテストできることを知っておいてください!
        assert_eq!(bad_add(1, 2), 3);
    }
}
```

`cargo test`でテストを走らせることができます。

```shell
$ cargo test

running 2 tests
test tests::test_bad_add ... FAILED
test tests::test_add ... ok

failures:

---- tests::test_bad_add stdout ----
        thread 'tests::test_bad_add' panicked at 'assertion failed: `(left == right)`
  left: `-1`,
 right: `3`', src/lib.rs:21:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.


failures:
    tests::test_bad_add

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

## テストと`?`
このテストは返り値を持ちませんでした。しかし、Rust 2018では、ユニットテストは
`?`を使うための`Result<()>`を返すことができます。これを使えば、簡潔にテストが
書けます。

```rust,editable
fn sqrt(number: f64) -> Result<f64, String> {
    if number >= 0.0 {
        Ok(number.powf(0.5))
    } else {
        Err("negative floats don't have square roots".to_owned())
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_sqrt() -> Result<(), String> {
        let x = 4.0;
        assert_eq!(sqrt(x)?.powf(2.0), x);
        Ok(())
    }
}
```

「[The Edition Guide][editionguide]」に詳細があります。

## テストとパニック

パニックするべき処理をテストするときは、`#[should_panic]`属性を使ってください。この属性は
オプションで引数`expected = `とパニックしたときのメッセージを渡すことができます。もし関数が
複数のパニックをするときは、ここで正しいパニックを指定することを忘れないでください。

```rust,ignore
pub fn divide_non_zero_result(a: u32, b: u32) -> u32 {
    if b == 0 {
        panic!("Divide-by-zero error");
    } else if a < b {
        panic!("Divide result is zero");
    }
    a / b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_divide() {
        assert_eq!(divide_non_zero_result(10, 2), 5);
    }

    #[test]
    #[should_panic]
    fn test_any_panic() {
        divide_non_zero_result(1, 0);
    }

    #[test]
    #[should_panic(expected = "Divide result is zero")]
    fn test_specific_panic() {
        divide_non_zero_result(1, 10);
    }
}
```

これらのテストを実行すると:

```shell
$ cargo test

running 3 tests
test tests::test_any_panic ... ok
test tests::test_divide ... ok
test tests::test_specific_panic ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## 特定のテストを実行する

`cargo test`コマンドにテストの名前を渡すことで、特定のテストを実行できます。

```shell
$ cargo test test_any_panic
running 1 test
test tests::test_any_panic ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

名前の一部を指定すると、それにマッチするすべてのテストが実行できます。

```shell
$ cargo test panic
running 2 tests
test tests::test_any_panic ... ok
test tests::test_specific_panic ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## テストを無視する

テストに`#[ignore]`属性を付けることでテストを無視することができます。そして、
`cargo test -- --ignored`コマンドで実行することができます。

```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 2), 4);
    }

    #[test]
    fn test_add_hundred() {
        assert_eq!(add(100, 2), 102);
        assert_eq!(add(2, 100), 102);
    }

    #[test]
    #[ignore]
    fn ignored_test() {
        assert_eq!(add(0, 0), 0);
    }
}
```

```shell
$ cargo test
running 3 tests
test tests::ignored_test ... ignored
test tests::test_add ... ok
test tests::test_add_hundred ... ok

test result: ok. 2 passed; 0 failed; 1 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-ignore

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

$ cargo test -- --ignored
running 1 test
test tests::ignored_test ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-ignore

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

[attribute]: ../attribute.md
[panic]: ../std/panic.md
[macros]: ../macros.md
[mod]: ../mod.md
[editionguide]: https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/question-mark-in-main-and-tests.html
