# ドメイン固有言語(DSL)

DSLはRustのマクロによって組み込まれた小さな「言語」です。これは、マクロシステムは普通のRust構文
へ展開しますが、これが小さな言語のように見えることによって成り立っています。これによって、特定の
用途において簡潔で直感的な構文を定義できます(適度に)。

ここで、小さな電卓APIを作ろうと思います。これは式を実行して、
その結果をコンソールに出力するものです。

```rust,editable
macro_rules! calculate {
    (eval $e:expr) => {{
        {
            let val: usize = $e; // 強制的に型を整数にする。
            println!("{} = {}", stringify!{$e}, val);
        }
    }};
}

fn main() {
    calculate! {
        eval 1 + 2 // `eval`はRustのキーワードではありません!
    }

    calculate! {
        eval (1 + 2) * (3 / 4)
    }
}
```

出力:

```txt
1 + 2 = 3
(1 + 2) * (3 / 4) = 0
```

これはとてもシンプルな例ですが、[`lazy_static`](https://crates.io/crates/lazy_static)や
[`clap`](https://crates.io/crates/clap)のようなもっと複雑な例もあります。

さらに、マクロ内に2重の波括弧があることに注意してください。外側のものは
`()`や`[]`のような、`macro_rules!`の構文です。
