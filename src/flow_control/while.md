# while

`while`キーワードは条件が真である間ループするのに使います。

悪名高き[FizzBuzz][fizzbuzz]を`while`ループで書いてみましょう。

```rust,editable
fn main() {
    // カウンタ変数
    let mut n = 1;

    // `n`が101より小さい間ループする
    while n < 101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }

        // カウンタをインクリメントする
        n += 1;
    }
}
```

[fizzbuzz]: https://en.wikipedia.org/wiki/Fizz_buzz
