# 可変性

可変なデータは`&mut T`で可変的に借用することができます。これ
*可変参照*と呼ばれ、借用した人に読み書きを許可します。
`&T`は不変なデータの参照をとり、借用した人はデータの読み取りは
できても、変更はできません。

```rust,editable,ignore,mdbook-runnable
#[allow(dead_code)]
#[derive(Clone, Copy)]
struct Book {
    // `&'static str`は読み取り専用メモリに確保した文字列の参照
    author: &'static str,
    title: &'static str,
    year: u32,
}

// この関数はbookの参照をとる
fn borrow_book(book: &Book) {
    println!("I immutably borrowed {} - {} edition", book.title, book.year);
}

// この関数はbookのか変参照をとり、`year`を2014に変更する
fn new_edition(book: &mut Book) {
    book.year = 2014;
    println!("I mutably borrowed {} - {} edition", book.title, book.year);
}

fn main() {
    // 不変なBook、`immutabook`を作る
    let immutabook = Book {
        // `&'static str`に格納される
        author: "Douglas Hofstadter",
        title: "Gödel, Escher, Bach",
        year: 1979,
    };

    // `immutabook`の可変なコピー、`mutabook`を作る
    let mut mutabook = immutabook;
    
    // 不変なオブジェクトを不変的に借用する
    borrow_book(&immutabook);

    // 可変なオブジェクトを不変的に借用する
    borrow_book(&mutabook);
    
    // 可変なオブジェクトを可変的に借用する
    new_edition(&mut mutabook);
    
    // エラー! 不変なオブジェクトは可変参照できません
    new_edition(&mut immutabook);
    // FIXME ^ この行をコメントアウトする
}
```

### See also:
[`static`][static]

[static]: ../lifetime/static_lifetime.md
