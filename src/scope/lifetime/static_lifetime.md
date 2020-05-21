# 静的ライフタイム

Rustはいくつかの定義済みのライフタイムを持ちます。その中の一つが
`'static`です。このような2つの状況に出会うかもしれません。

```rust, editable
// 'staticライフタイムで参照する
let s: &'static str = "hello world";

// トレイト境界としての'static
fn generic<T>(x: T) where T: 'static {}
```

両者は関係していますが、微妙に異なります。Rustを学ぶ際に混同してしまうもの
の一つです。それぞれについて例を見ていきましょう。

## 参照のライフタイム

参照のライフタイムとしての`'static`は、プログラムが走っている間中有効なデータ
への参照を表します。これを短いライフタイムに圧縮することもできます。

`'static`ライフタイムの変数を作る方法は2つありますが、両方とも
バイナリの読み取り専用メモリに保存されます。

* `static`宣言で定数を作る。
* `&'static str`型の文字列リテラルを作る。

以下の例はそれぞれの方法を示しています。

```rust,editable
// `'static`ライフタイムの定数を作る。
static NUM: i32 = 18;

// `NUM`の参照を`'static`ライフタイムを入力のライフタイムに圧縮して
// 返す。
fn coerce_static<'a>(_: &'a i32) -> &'a i32 {
    &NUM
}

fn main() {
    {
        // `string`リテラルを作ってプリントする。
        let static_string = "I'm in read-only memory";
        println!("static_string: {}", static_string);

        // `static_string`がスコープを出た時、参照は使えなくなりますが、
        // データはバイナリに残ります。
    }

    {
        // `coerce_static`に使う整数を作る。
        let lifetime_num = 9;

        // `NUM`を`lifetime_num`のライフタイムに圧縮する
        let coerced_static = coerce_static(&lifetime_num);

        println!("coerced_static: {}", coerced_static);
    }

    println!("NUM: {} stays accessible!", NUM);  // NUM: {}はまだアクセスできます。
}
```

## トレイトの境界

トレイトの境界として使った時、静的でない参照を含むことができないことを
意味します。つまり、受け取り手はその型の変数を好きなだけ保持することができ、
dropするまで無効にならないということです。

所有しているすべてのデータは`'staic`ライフタイム境界を持ちますが、その参照は
持ちません。

```rust,editable,compile_fail
use std::fmt::Debug;

fn print_it( input: impl Debug + 'static )
{
    println!( "'static value passed in is: {:?}", input );
}

fn use_it()
{
    // iは所有していて、参照を持たないので'staticです。
    let i = 5;
    print_it(i);

    // &iはuse_it()で定義されたスコープでしか使えないので、
    // 'staticではありません。
    print_it(&i);
}
```
コンパイラはこのように出力します。
```ignore
error[E0597]: `i` does not live long enough
  --> src/lib.rs:15:15
   |
15 |     print_it(&i);
   |     ---------^^--
   |     |         |
   |     |         borrowed value does not live long enough
   |     argument requires that `i` is borrowed for `'static`
16 | }
   | - `i` dropped here while still borrowed
```

### こちらも参照:

- [`'static`定数][static_const]

[static_const]: ../../custom_types/constants.md
