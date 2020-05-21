# マクロ指定子

マクロの引数は`$`で始まり、そのタイプは*指定子*で注釈します。

```rust,editable
macro_rules! create_function {
    // このマクロは`ident`指定子の引数をとり、
    // `$func_name`という名前の関数を作ります。
    // The `ident` designator is used for variable/function names.
    ($func_name:ident) => {
        fn $func_name() {
            // `stringify!`マクロは`ident`を文字列に変換します。
            println!("You called {:?}()",
                     stringify!($func_name));
        }
    };
}

// 上のマクロを使って、`foo`と`bar`という関数を定義します。
create_function!(foo);
create_function!(bar);

macro_rules! print_result {
    // このマクロは式`$expression`をとり、それを文字列にしたものと
    // その結果を返します。`expr`指定子は式に対して指定します。
    ($expression:expr) => {
        // `stringify!`は式を*そのまま*文字列に変換します。
        println!("{:?} = {:?}",
                 stringify!($expression),
                 $expression);
    };
}

fn main() {
    foo();
    bar();

    print_result!(1u32 + 1);

    // ブロックも式です!
    print_result!({
        let x = 1u32;

        x * x + 2 * x - 1
    });
}
```

これらが利用可能な指定子の一例です。

* `block`
* `expr`は式に使います。
* `ident`は変数/関数名に使います。
* `item`
* `literal`はリテラルに使います
* `pat` (*パターン*)
* `path`
* `stmt` (*式*)
* `tt` (*トークン木*)
* `ty` (*型*)
* `vis` (*可視性指定子*(`pub`など))

完全な一覧は[Rustリファレンス][Rust Reference]を参照してください。

[Rust Reference]: https://doc.rust-lang.org/reference/macros-by-example.html
