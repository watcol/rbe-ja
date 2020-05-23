# パス

`Path`構造体はファイルシステム下のファイルパスを格納します。これにはUNIX系
システム用の`posix::Path`と、Windows用の`windows::Path`の2つがあります。
これらは適切なプラットフォーム依存の`Path`列挙子を提供します。

`Path`は`OsStr`から作ることができ、ファイルやディレクトリに対する情報
を操作するいくつかのメソッドを持っています。

`Path`は内部的にはUTF-8文字列として保存されず、バイト列(`Vec<u8>`)として
保存されることに注意してください。そのため、`Path`から`&str`への変換は
失敗する可能性があります(`Option`が返されます)。

```rust,editable
use std::path::Path;

fn main() {
    // `&'static str`から`Path`を作る
    let path = Path::new(".");

    // `display`メソッドは`Show`できる構造体を返します。
    let _display = path.display();

    // `join`はパスをOS依存のセパレータで結合し、
    // 新しいパスを返します。
    let new_path = path.join("a").join("b");

    // パスを文字列スライスに変換する。
    match new_path.to_str() {
        None => panic!("new path is not a valid UTF-8 sequence"),
        Some(s) => println!("new path is {}", s),
    }
}

```

他の`Path`(`posix::Path`または`windows::Path`)のメソッドや
`Metadata`構造体についても確認してください。

### こちらも参照:

- [OsStr][1]
- [Metadata][2]

[1]: https://doc.rust-lang.org/std/ffi/struct.OsStr.html
[2]: https://doc.rust-lang.org/std/fs/struct.Metadata.html
