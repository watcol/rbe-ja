# イテレータを検索する

`Iterator::find`はイテレータ内で条件を満たす関数を検索する関数です。もし
一つも条件を満たさなかったときは、`None`を返します。型シグネチャは以下の
通りです。

```rust,ignore
pub trait Iterator {
    // 要素の型
    type Item;

    // `find`は`&mut self`をとります。よって呼び出し元は借用され、変更される
    // かもしれませんが、消費はされません。
    fn find<P>(&mut self, predicate: P) -> Option<Self::Item> where
        // `FnMut`はキャプチャする値が変更される可能性はあるが、
        // 消費はされないことを表します。`&Self::Item`はクロージャに
        // 参照を渡すことを意味します。
        P: FnMut(&Self::Item) -> bool {}
}
```

```rust,editable
fn main() {
    let vec1 = vec![1, 2, 3];
    let vec2 = vec![4, 5, 6];

    // ベクターの`iter()`は`&i32`を産出します。
    let mut iter = vec1.iter();
    // ベクターの`into_iter()`は`i32`を産出します。
    let mut into_iter = vec2.into_iter();

    // ベクターの`iter()`は`&i32`を産出します。要素の参照をとる必要があるので、
    // `&&i32`を`i32`に分割代入する必要があります。
    println!("Find 2 in vec1: {:?}", iter     .find(|&&x| x == 2));
    // `into_iter()`は`i32`を産出します。なので同様に、`&i32`を`i32`
    // に分割代入する必要があります。
    println!("Find 2 in vec2: {:?}", into_iter.find(| &x| x == 2));

    let array1 = [1, 2, 3];
    let array2 = [4, 5, 6];

    // 配列の`iter()`は`&i32`を産出します。
    println!("Find 2 in array1: {:?}", array1.iter()     .find(|&&x| x == 2));
    // 配列の`into_iter()`も`&i32`を産出します。
    println!("Find 2 in array2: {:?}", array2.into_iter().find(|&&x| x == 2));
}
```

`Iterator::find`は要素の参照を渡します。要素の_インデックス_がほしいときは
`Iterator::position`を使ってください。

```rust,editable
fn main() {
    let vec = vec![1, 9, 3, 3, 13, 2];

    let index_of_first_even_number = vec.iter().position(|x| x % 2 == 0);
    assert_eq!(index_of_first_even_number, Some(5));
    
    
    let index_of_first_negative_number = vec.iter().position(|x| x < &0);
    assert_eq!(index_of_first_negative_number, None);
}
```

### こちらも参照:

- [`std::iter::Iterator::find`][find]
- [`std::iter::Iterator::find_map`][find_map]
- [`std::iter::Iterator::position`][position]
- [`std::iter::Iterator::rposition`][rposition]

[find]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.find
[find_map]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.find_map
[position]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.position
[rposition]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.rposition
