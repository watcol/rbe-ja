# テストケース: リスト

要素が順番に処理されていくような構造体に対して`fmt::Display`を実装するのは
トリッキーです。なぜなら、それぞれの`write!`が`fmt::Result`を返すからです。
適切に処理するには*すべての*Resultに対して処理を行わなければいけません。
Rust はこのような目的のために`?`演算子を用意しています。

次のように`?`を`write!`に対して使えます。

```rust,ignore
// `write!`を実行し、エラーが出た場合はerrorを返し、
// そうでなければ処理を続行する。
write!(f, "{}", value)?;
```

もう1つ、同じように動く`try!`マクロを使うこともできます。
これは少し冗長であり、もはや推奨されていませんが、古い
Rustのコードを読むときは。このように`try!`を使っています。

```rust,ignore
try!(write!(f, "{}", value));
```

`?`を使うことができれば、`fmt::Display`を`Vec`に実装する
のはより簡単です。

```rust,editable
use std::fmt; // `fmt`モジュールをインポートする。

// `vec`を含む構造体`List`を実装する。
struct List(Vec<i32>);

impl fmt::Display for List {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // タプルインデックスで要素を展開し、
        // `vec`という参照を作る。
        let vec = &self.0;

        write!(f, "[")?;

        // `v`で`vec`を反復し、enumerateで
        // `count`にカウントを取得する。
        for (count, v) in vec.iter().enumerate() {
            // 最初の要素以外、全ての要素の前にコンマをつける。
            // ?演算子かtry!を使って、エラーのときにそれを返す。
            if count != 0 { write!(f, ", ")?; }
            write!(f, "{}", v)?;
        }

        // 開いた括弧を閉じ、fmt::Resultを返す。
        write!(f, "]")
    }
}

fn main() {
    let v = List(vec![1, 2, 3]);
    println!("{}", v);
}
```

### 演習

ベクタのインデックスも表示するようにプログラムを変更しましょう。
新しい出力は以下のようになるはずです。

```rust,ignore
[0: 1, 1: 2, 2: 3]
```

### こちらも参照:

[`for`][for], [`ref`][ref], [`Result`][result], [`struct`][struct],
[`?`][q_mark], and [`vec!`][vec]

[for]: ../../../flow_control/for.md
[result]: ../../../std/result.md
[ref]: ../../../scope/borrow/ref.md
[struct]: ../../../custom_types/structs.md
[q_mark]: ../../../std/result/question_mark.md
[vec]: ../../../std/vec.md
