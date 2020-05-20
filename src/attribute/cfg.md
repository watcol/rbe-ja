# `cfg`

条件付きコンパイルは次の2つのオペレータによって行われます。

* `cfg`属性: 属性としての`#[cfg(...)]`
* `cfg!`マクロ: 真偽値式を使ったマクロ`cfg!(...)`

後者は実行時にチェックされ、`true`や`false`などのリテラルも使えます。
どちらも適切な構文で記述する必要があります。

```rust,editable
// この関数はターゲットOSがlinuxである場合のみコンパイルされます
#[cfg(target_os = "linux")]
fn are_you_on_linux() {
    println!("You are running linux!");
}

// この関数はターゲットOSがlinuxで*ない*場合のみコンパイルされます
#[cfg(not(target_os = "linux"))]
fn are_you_on_linux() {
    println!("You are *not* running linux!");
}

fn main() {
    are_you_on_linux();

    println!("Are you sure?");
    if cfg!(target_os = "linux") {
        println!("Yes. It's definitely linux!");
    } else {
        println!("Yes. It's definitely *not* linux!");
    }
}
```

### こちらも参照:

- [参照][ref]
- [`cfg!`][cfg]
- [マクロ][macros]

[cfg]: https://doc.rust-lang.org/std/macro.cfg!.html
[macros]: ../macros.md
[ref]: https://doc.rust-lang.org/reference/attributes.html#conditional-compilation
