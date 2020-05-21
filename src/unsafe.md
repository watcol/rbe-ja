# 安全でない操作

この節では、まず[公式ドキュメント][unsafe]から、「one should try to minimize
the amount of unsafe code in a code base.」(安全でないコードがコードベースに
しめる割合が最小になるよう努力しなければならない。)これを念頭に置いて、始めましょう。
Rustの`unsafe`アノテーションは、コンパイラによる保護を無効化するのに使います。
`unsafe`はおもにこれらの4つのことに使われます。

* 生ポインタを参照する
* `unsafe`な関数やメソッドを呼び出す。(FFIから関数を呼び出すときも含みます。
  この本の[前の章](std_misc/ffi.md)も参照してください。) 
* 静的可変変数を変更する
* 安全でないトレイトを実装する

### 生ポインタ
生ポインタ`*`と参照`&T`は似ていますが、参照は有効なデータを参照していることが
ボローチェッカーによっていつも保証されています。生ポインタを参照するには`unsafe`
ブロックを使う必要があります。

```rust,editable
fn main() {
    let raw_p: *const u32 = &10;

    unsafe {
        assert!(*raw_p == 10);
    }
}
```

### 安全でない関数を呼び出す
いくつかの関数は`unsafe`ブロックで定義されていて、これはコンパイラの代わりに
プログラマが正確性を管理しなければいけないことを意味します。一つの例は、最初の
要素のポインタと長さからスライスを作る[`std::slice::from_raw_parts`]関数です。

```rust,editable
use std::slice;

fn main() {
    let some_vector = vec![1, 2, 3, 4];

    let pointer = some_vector.as_ptr();
    let length = some_vector.len();

    unsafe {
        let my_slice: &[u32] = slice::from_raw_parts(pointer, length);

        assert_eq!(some_vector.as_slice(), my_slice);
    }
}
```

`slice::from_raw_parts`は、メモリのポインタが有効で、かつすべて正しい型を持っている
という一種の仮定に基づいています。これはいつも成り立つとは限らず、もし成り立たなかった
ときの動作が未定義で、何が起こるかわかりません。

[unsafe]: https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html
[`std::slice::from_raw_parts`]: https://doc.rust-lang.org/std/slice/fn.from_raw_parts.html
