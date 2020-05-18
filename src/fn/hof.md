# 高階関数

Rustは高階関数(HOF)を提供しています。これは1つ以上の関数を引数にとることで、
より使いやすい関数を作る機能です。高階関数と遅延イテレータはRustの関数型機能です。

```rust,editable
fn is_odd(n: u32) -> bool {
    n % 2 == 1
}

fn main() {
    println!("Find the sum of all the squared odd numbers under 1000");  // 1000以下の奇数かつ平方数の和を求めます
    let upper = 1000;

    // 命令型のアプローチ
    // 蓄積用変数を定義する
    let mut acc = 0;
    // 0, 1, 2, ... と無限まで繰り返す
    for n in 0.. {
        // 数を2乗する
        let n_squared = n * n;

        if n_squared >= upper {
            // 上限を超えれば終了する
            break;
        } else if is_odd(n_squared) {
            // 奇数ならばaccに追加する
            acc += n_squared;
        }
    }
    println!("imperative style: {}", acc);  // 命令型: {}

    // 関数型のアプローチ
    let sum_of_squared_odd_numbers: u32 =
        (0..).map(|n| n * n)                             // すべての自然数を2乗する
             .take_while(|&n_squared| n_squared < upper) // 上限まで
             .filter(|&n_squared| is_odd(n_squared))     // 奇数ならば
             .fold(0, |acc, n_squared| acc + n_squared); // 合計する
    println!("functional style: {}", sum_of_squared_odd_numbers); // 関数型: {}
}
```

[Option型][option]と[イテレータ][iter]の実装には高階関数が使われています。

[option]: https://doc.rust-lang.org/core/option/enum.Option.html
[iter]: https://doc.rust-lang.org/core/iter/trait.Iterator.html
