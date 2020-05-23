# 外部関数インターフェース

RustはC言語のライブラリの外部関数インターフェース(FFI)を提供しています。外部の
関数は`extern`ブロックで宣言され、そのブロックにはライブラリ名を付けた`#[link]`
属性が与えられている必要があります。

```rust,ignore
use std::fmt;

// このブロックはlibmとリンクします。
#[link(name = "m")]
extern {
    // これは外部関数です。
    // これは単精度複素数の平方根を求めます。
    fn csqrtf(z: Complex) -> Complex;

    fn ccosf(z: Complex) -> Complex;
}

// 外部関数は安全でない可能性があるので、安全なラッパー
// 関数が必要です。
fn cos(z: Complex) -> Complex {
    unsafe { ccosf(z) }
}

fn main() {
    // z = -1 + 0i
    let z = Complex { re: -1., im: 0. };

    // 外部関数の呼び出しはunsafeです。
    let z_sqrt = unsafe { csqrtf(z) };

    println!("the square root of {:?} is {:?}", z, z_sqrt);

    // unsafeをラップした安全なAPIを呼び出す。
    println!("cos({:?}) = {:?}", z, cos(z));
}

// 単精度複素数の最小実装
#[repr(C)]
#[derive(Clone, Copy)]
struct Complex {
    re: f32,
    im: f32,
}

impl fmt::Debug for Complex {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        if self.im < 0. {
            write!(f, "{}-{}i", self.re, -self.im)
        } else {
            write!(f, "{}+{}i", self.re, self.im)
        }
    }
}
```
