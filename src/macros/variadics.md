# 可変長引数

可変長引数は様々な数の引数をとります。例えば、`println!`は、フォーマット文字列
に従って、いろいろな数の引数をとります。

前の章の`calculate!`マクロを拡張してみましょう。

```rust,editable
macro_rules! calculate {
    // `eval`がひとつだけの場合
    (eval $e:expr) => {{
        {
            let val: usize = $e; // Force types to be integers
            println!("{} = {}", stringify!{$e}, val);
        }
    }};

    // `eval`が複数ある場合、再帰を使う。
    (eval $e:expr, $(eval $es:expr),+) => {{
        calculate! { eval $e }
        calculate! { $(eval $es),+ }
    }};
}

fn main() {
    calculate! { // 可変長の`calculate!`です!
        eval 1 + 2,
        eval 3 + 4,
        eval (2 * 3) + 1
    }
}
```

出力:

```txt
1 + 2 = 3
3 + 4 = 7
(2 * 3) + 1 = 7
```
