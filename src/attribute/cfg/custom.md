# カスタマイズ

`target_os`など、いくつかの条件は`rustc`によって提供されますが、`ructc`に
`--cfg`フラグを追加することで指定できるカスタム条件もあります。

```rust,editable,ignore,mdbook-runnable
#[cfg(some_condition)]
fn conditional_function() {
    println!("condition met!");
}

fn main() {
    conditional_function();
}
```

`cfg`フラグがないとどうなるか試してみてください。

`cfg`フラグを指定すると:

```shell
$ rustc --cfg some_condition custom.rs && ./custom
condition met!
```
