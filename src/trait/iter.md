# イテレータ

[`Iterator`][iter]トレイトは配列のようなコレクションにイテレータを実装するトレイトです。

このトレイトは次の要素を表す`next`メソッドのみ実装すればよく、これは`impl`ブロックで
実装するか、(配列やrangeのように)もう実装されていることもあります。

一般的なシチュエーションでの利便性のため、`for`は自動的にコレクションに対して
[`.into_iter()`][intoiter]メソッドを使い、イテレータを作ります。

```rust,editable
struct Fibonacci {
    curr: u32,
    next: u32,
}

// `Fibonacci`に`Iterator`を実装する。
// `Iterator`トレイトは、次の要素を返す`next`メソッドのみ実装されていれば良い。
impl Iterator for Fibonacci {
    type Item = u32;
    
    // ここで、`.curr`と`.next`を使って数列を作り、
    // `Option<T>`型を返します。
    //     * `Iterator`が終われば、`None`を返します。
    //     * そうでなければ、次の値を`Some`でくるみ、返却します。
    fn next(&mut self) -> Option<u32> {
        let new_next = self.curr + self.next;

        self.curr = self.next;
        self.next = new_next;

        // フィボナッチ数列には終わりは無いため、`Iterator`は`None`を
        // 返さず、いつも`Some`を返します。
        Some(self.curr)
    }
}

// フィボナッチ数列生成器を返す。
fn fibonacci() -> Fibonacci {
    Fibonacci { curr: 0, next: 1 }
}

fn main() {
    // `0..3`は0、1、2からなるイテレータを生成する
    let mut sequence = 0..3;

    println!("Four consecutive `next` calls on 0..3");  // 0..3から4回連続で`next`を呼ぶ。
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());

    // `for`はイテレータを`None`が出るまで続ける。
    // それぞれの`Some`は変数(ここでは`i`)に分割代入される。
    println!("Iterate through 0..3 using `for`");  // `for`を使って0..3に対して繰り返し処理をする。
    for i in 0..3 {
        println!("> {}", i);
    }

    // `take(n)`メソッドは最初`n`回分のイテレータを返す
    println!("The first four terms of the Fibonacci sequence are: ");  // フィボナッチ数列の最初の4つは:
    for i in fibonacci().take(4) {
        println!("> {}", i);
    }

    // `skip(n)`メソッドは最初の`n`回をなくしたイテレータを返す。
    println!("The next four terms of the Fibonacci sequence are: ");  // フィボナッチ数列の次の4つは:
    for i in fibonacci().skip(4).take(4) {
        println!("> {}", i);
    }

    let array = [1u32, 3, 3, 7];

    // `iter`メソッドで配列、スライスからイテレータを生成する。
    println!("Iterate the following array {:?}", &array);  // 以下の配列{:?}をイテレートする
    for i in array.iter() {
        println!("> {}", i);
    }
}
```

[intoiter]: https://doc.rust-lang.org/std/iter/trait.IntoIterator.html
[iter]: https://doc.rust-lang.org/core/iter/trait.Iterator.html
