# ファイル階層

モジュールはファイル/ディレクトリ階層で表すことができます。
[可視性の例][visibility]をファイルで再現してみましょう。

```shell
$ tree .
.
|-- my
|   |-- inaccessible.rs
|   |-- mod.rs
|   `-- nested.rs
`-- split.rs
```

`split.rs`の中身:

```rust,ignore
// この宣言は`my.rs`または`my/mod.rs`を探索し、そこにあった要素を
// `my`モジュールとしてスコープに導入します。
mod my;

fn function() {
    println!("called `function()`");
}

fn main() {
    my::function();

    function();

    my::indirect_access();

    my::nested::function();
}

```

`my/mod.rs`の中身:

```rust,ignore
// 同様に`mod inaccessible`と`mod nested`は`nested.rs`や`inaccessible.rs`ファイルを
// 探索し、そのモジュールを導入します。
mod inaccessible;
pub mod nested;

pub fn function() {
    println!("called `my::function()`");
}

fn private_function() {
    println!("called `my::private_function()`");
}

pub fn indirect_access() {
    print!("called `my::indirect_access()`, that\n> ");

    private_function();
}
```

`my/nested.rs`の中身:

```rust,ignore
pub fn function() {
    println!("called `my::nested::function()`");
}

#[allow(dead_code)]
fn private_function() {
    println!("called `my::nested::private_function()`");
}
```

`my/inaccessible.rs`の中身:

```rust,ignore
#[allow(dead_code)]
pub fn public_function() {
    println!("called `my::inaccessible::public_function()`");
}
```

同じように動くか確かめましょう。

```shell
$ rustc split.rs && ./split
called `my::function()`
called `function()`
called `my::indirect_access()`, that
> called `my::private_function()`
called `my::nested::function()`
```

[visibility]: visibility.md
