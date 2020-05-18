# 関数

関数は`fn`キーワードで宣言できます。引数の型は変数のように注釈し、
関数が値を返すときは、その型を矢印`->`の後に指定する必要があります。

関数内の最後の式が返されます。また、`return`文でループや`if`の中など、
関数の途中で値を返すことができます。

FizzBuzzを関数を使って書き直してみましょう!

```rust,editable
// C/C++と違い、関数定義の順番に決まりはないです。
fn main() {
    // ここで関数を使い、あとで宣言することができます。
    fizzbuzz_to(100);
}

// 真偽値を返す関数。
fn is_divisible_by(lhs: u32, rhs: u32) -> bool {
    // コーナーケースでは、関数が終わる前に値を返します。
    if rhs == 0 {
        return false;
    }

    // これは式であり、ここには`return`キーワードが必要ありません。
    lhs % rhs == 0
}

// 値を返さない関数です。実際にはユニット型`()`を返しています。
fn fizzbuzz(n: u32) -> () {
    if is_divisible_by(n, 15) {
        println!("fizzbuzz");
    } else if is_divisible_by(n, 3) {
        println!("fizz");
    } else if is_divisible_by(n, 5) {
        println!("buzz");
    } else {
        println!("{}", n);
    }
}

// 関数が`()`を返す時、返り値の型は省略できます。
fn fizzbuzz_to(n: u32) {
    for n in 1..n + 1 {
        fizzbuzz(n);
    }
}
```
