# `dead_code`

コンパイラは使っていない関数に対して警告する
`dead_code`[*リント*][lint]を提供します。これを無効化
するための*属性*があります。

```rust,editable
fn used_function() {}

// `#[allow(dead_code)]`は`dead_code`リントを無効化します
#[allow(dead_code)]
fn unused_function() {}

fn noisy_unused_function() {}
// FIXME ^ 警告を消すために属性を加えてください

fn main() {
    used_function();
}
```

実用のプログラムでは使わない関数は取り除いたほうが良いです。しかし、このような
サンプルでは自然なサンプルになるように所々でこの属性を使います。

[lint]: https://en.wikipedia.org/wiki/Lint_%28software%29
