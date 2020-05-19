# Iterator::any

`Iterator::any`はイテレータ内に一つでも条件を満たす要素があれば`true`を、
なければ`false`を返すメソッドです。

```rust,ignore
pub trait Iterator {
    // イテレータ内の要素の型。
    type Item;

    // `any`は`&mut self`をとるため、呼び出し元を借用し、変更するかも
    // しれませんが、消費はしません。
    fn any<F>(&mut self, f: F) -> bool where
        // `FnMut`はクロージャがキャプチャした値を変更するかもしれないが、
        // 消費はしないことを表します。`Self::Item`はクロージャに
        // 値を引数として渡すことを意味します。
        F: FnMut(Self::Item) -> bool {}
}
```

```rust,editable
fn main() {
    let vec1 = vec![1, 2, 3];
    let vec2 = vec![4, 5, 6];

    // ベクターの`iter()`は`&i32`を産出するので、`i32`に分割代入する必要があります。
    println!("2 in vec1: {}", vec1.iter()     .any(|&x| x == 2));
    // ベクターの`into_iter()`は`i32`を産出するので、分割代入は必要ありません。
    println!("2 in vec2: {}", vec2.into_iter().any(| x| x == 2));

    let array1 = [1, 2, 3];
    let array2 = [4, 5, 6];

    // 配列の`iter()`は`&i32`を産出します。
    println!("2 in array1: {}", array1.iter()     .any(|&x| x == 2));
    // 配列の`into_iter()`もなんと`&i32`を産出します。
    println!("2 in array2: {}", array2.into_iter().any(|&x| x == 2));
}
```

### こちらも参照:

[`std::iter::Iterator::any`][any]

[any]: https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.any
