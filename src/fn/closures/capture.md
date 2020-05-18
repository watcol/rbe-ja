# キャプチャ

クロージャは柔軟で、型注釈がなくても動きます。これのおかげで、
場合に応じて所有権をもらったり借用したりすることができます。
クロージャは変数を捕捉(キャプチャ)できます。

* 参照として(`&T`)
* 可変参照として(`&mut T`)
* 値として(`T`)

クロージャははじめ参照を取得しようとし、場合に応じて他のものも取得します。

```rust,editable
fn main() {
    use std::mem;

    let color = String::from("green");

    // `color`を借用(`&`)して出力するクロージャを作り、借用と共に変数
    // `print`に保存します。`print`が最後に使われるまで借用は続きます。
    // `println!`は不変参照があれば機能するので、これ以上なにかする必要はない。
    let print = || println!("`color`: {}", color);

    // 借用を使ったクロージャを呼び出す。
    print();

    // クロージャは不変な借用しか保持していないため、
    // `color`はもう1度借用できます。
    let _reborrow = &color;
    print();

    // `print`の最後の使用のあとでのみ所有権を渡せます。
    let _color_moved = color;


    let mut count = 0;
    // `count`をインクリメントするクロージャは`&mut count`または`count`を
    // 必要としますが、`&mut count`で事足りるのでそれを使います。よって
    // `count`を借用します。
    //
    // 内部に可変な型があるので、`inc`には`mut`が必要です。
    // 可変なクロージャは呼び出す度に内部変数を変更します。
    let mut inc = || {
        count += 1;
        println!("`count`: {}", count);
    };

    // 可変借用を使ったクロージャを呼ぶ。
    inc();

    // あとで呼び出されるため、クロージャはまだ`count`を可変借用しています。
    // なので、もう1度借用とするとエラーになります。
    // let _reborrow = &count; 
    // ^ TODO: この行をアンコメントしてみてください。
    inc();

    // クロージャはもう使われないので、`&mut count`を借用する必要はありません。
    // なので、エラーなく再借用できます。
    let _count_reborrowed = &mut count; 

    
    // コピーできない型
    let movable = Box::new(3);

    // `mem::drop`は`T`を必要とするので、値そのものが必要である。
    // コピーできる型はコピーし、できない型は値をそのまま移動させます。
    let consume = || {
        println!("`movable`: {:?}", movable);
        mem::drop(movable);
    };

    // `consume`は変数を消費するため、一度しか呼べません。
    consume();
    // consume();
    // ^ TODO: この行をアンコメントしてみてください。
}
```

バーティカルバー(`|`)の前に`move`を付けると、キャプチャした変数の
所有権をもらいます。

```rust,editable
fn main() {
    // `Vec`はコピーできない型です。
    let haystack = vec![1, 2, 3];

    let contains = move |needle| haystack.contains(needle);

    println!("{}", contains(&1));
    println!("{}", contains(&4));

    // println!("There're {} elements in vec", haystack.len());
    // ^ ボローチェッカーは移動させた後の変数の再利用を禁止しているので、
    // 上をアンコメントすると、コンパイルエラーになります。
    
    // `move`をクロージャから削除すると、クロージャは変数haystackを不変借用するので、
    // haystackはまだ使え、上をアンコメントしてもエラーしなくなります。
}
```

### こちらも参照:

- [`Box`][box]
- [`std::mem::drop`][drop]

[box]: ../../std/box.md
[drop]: https://doc.rust-lang.org/std/mem/fn.drop.html
