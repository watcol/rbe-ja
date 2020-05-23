# テストケース: map-reduce

Rustを使えば、苦労なしに簡単に並行プログラミングできます。

標準ライブラリはスレッド管理のための素晴らしい機能を提供しています。
これらは、Rustの所有権やエイリアスの概念と協調した、データ競合の少ない
並行化を可能にします。

エイリアシングルール(一つの他と共存できない可変参照と、他といくつでも共存できる不変参照)によって、
他のスレッドとの競合を原理的になくすことができます。(同期したいときは、そのための`Mutex`や`Channnel`
といった型が存在します。)

この例では、ブロック内のすべての数字を加算します。ここでは、ブロックをチャンクに分割して、それぞれの
チャンクの計算を別のスレッドで行っています。それぞれのスレッドが合計を計算したら、即座にそれぞれの
スレッドの持つ数を合算します。

スレッドを超えてデータを参照しても、コンパイラは読み取り専用の参照しか渡していないことを知っているので、
安全でない操作やデータ競合が起こらないことに注意してください。また、データをスレッドに`move`しても、
スレッドが終了するまでデータを確保するので、危険なポインタもできません。

```rust,editable
use std::thread;

// `main`スレッド
fn main() {

    // これが処理するデータです。
    // map-reduceアルゴリズムでこれらの数字の合計を計算します。
    // 空白によってチャンクを分割できます。
    //
    // TODO: 空白を増やしたらどうなるか見てみましょう!
    let data = "86967897737416471853297327050364959
11861322575564723963297542624962850
70856234701860851907960690014725639
38397966707106094172783238747669219
52380795257888236525459303330302837
58495327135744041048897885734297812
69920216438980873548808413720956532
16278424637452589860345374828574668";

    // 子スレッドを保持するベクタ
    let mut children = vec![];

    /*************************************************************************
     * "Map"フェーズ
     *
     * データを分割し、最初の処理を行う
     ************************************************************************/

    // それぞれの計算のためにデータを分割する。
    // それぞれのチャンクは(&str)から本物のデータを参照できる。
    let chunked_data = data.split_whitespace();

    // データ単位ごとにイテレーションする
    // .enumerate()は現在のループインデックスとデータを
    // タプル"(index, element)"に入れ、即座に2つの変数
    // "i"と"data_segment"に「分割代入」する。
    for (i, data_segment) in chunked_data.enumerate() {
        println!("data segment {} is \"{}\"", i, data_segment);

        // それぞれのデータ単位を別々のスレッドで処理する。
        //
        // spawn()はスレッドの情報を返し、これは返り値にアクセス
        // するために保持しないといけない。
        //
        // 'move || -> u32'は、
        // * 引数を取らない ('||')
        // * キャプチャした変数を移動するする('move')
        // * 32ビット浮動小数点整数を返す('-> u32')
        // クロージャです。
        //
        // Rustは、クロージャ自体から'-> u32'を推論できるので
        // 省略することもできる。
        //
        // TODO: 'move'を外すとどうなるか見てみましょう。
        children.push(thread::spawn(move || -> u32 {
            // そのデータ単位の合計を計算する
            let result = data_segment
                        // 文字ごとにイテレートする..
                        .chars()
                        // ..文字を数値に変換する..
                        .map(|c| c.to_digit(10).expect("should be a digit"))
                        // ..すべての数値を合計する
                        .sum();

            // println!はstdoutをロックするので、テキストの競合は起こらない
            println!("processed segment {}, result={}", i, result);

            // は"式ベース言語"なので、"return"は必要なく、ブロックの最後の式が
            // 自動的に返される。
            result

        }));
    }


    /*************************************************************************
     * "Reduce"フェーズ
     *
     * それぞれの結果を取得し、合算する。
     ************************************************************************/

    // 新しいベクターにそれぞれの結果を入れる。
    let mut intermediate_sums = vec![];
    for child in children {
        // それぞれの子スレッドの返り値を取得する。
        let intermediate_sum = child.join().unwrap();
        intermediate_sums.push(intermediate_sum);
    }

    // すべての数値を合算して、最後の結果とする。
    //
    // "ターボフィッシュ"(::<>)を使うことでsum()に型のヒントを与える。
    //
    // TODO: ターボフィッシュを使わず、明示的に
    // final_resultの型を指定してください。
    let final_result = intermediate_sums.iter().sum::<u32>();

    println!("Final sum result: {}", final_result);
}


```

### 代入
ユーザー入力からスレッド数を決めるのは賢くありません。ユーザーがもしたくさんのスペースを入れたとして、
*本当に*2,000個のスレッドを作る必要があるでしょうか? いつも限られた数のチャンクを作るようにして、
その数を静的定数としてプログラムのはじめに定義するようにプログラムを変更してみてください。

### こちらも参照:
* [スレッド][thread]
* [ベクター][vectors]
* [イテレータ][iterators]
* [クロージャ][closures]
* [move][move]
* [クロージャの`move`][move_closure]
* [分割代入][destructuring]
* 型インターフェースを補助する[ターボフィッシュ][turbofish]
* [unwrapとexpect][unwrap]
* [enumerate][enumerate]

[thread]: ../threads.md
[vectors]: ../../std/vec.md
[iterators]: ../../trait/iter.md
[destructuring]: https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html#destructuring-to-break-apart-values
[closures]: ../../fn/closures.md
[move]: ../../scope/move.md
[move_closure]: https://doc.rust-lang.org/book/ch13-01-closures.html#closures-can-capture-their-environment
[turbofish]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect
[unwrap]: ../../error/option_unwrap.md
[enumerate]: https://doc.rust-lang.org/book/loops.html#enumerate
