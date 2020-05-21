# オーバーロード

マクロは違う引数を取るようにオーバーロードできます。
ついては、`macro_rules!`はmatchブロックのように使用できます。

```rust,editable
// `test!` will compare `$left` and `$right`
// in different ways depending on how you invoke it:
macro_rules! test {
    // 引数をコンマで区切る必要はありません。
    // テンプレートが使えます!
    ($left:expr; and $right:expr) => {
        println!("{:?} and {:?} is {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left && $right)
    };
    // ^ それぞれのアームはセミコロンで終わる必要があります。
    ($left:expr; or $right:expr) => {
        println!("{:?} or {:?} is {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left || $right)
    };
}

fn main() {
    test!(1i32 + 1 == 2i32; and 2i32 * 2 == 4i32);
    test!(true; or false);
}
```
