# 借用

ほとんどの場合、データの所有権を取らずにデータにアクセスしたいです。このために、
Rustは*借用*のメカニズムを使用しています。オブジェクトを値(`T`)で渡す代わりに、
参照(`&T`)で渡すことができます。

コンパイラは参照が*常に*有効なオブジェクトのポインタであることを(ボローチェッカー
によって)保証します。つまり、オブジェクトの参照が存在する限り、そのオブジェクトは
破棄されません。

```rust,editable,ignore,mdbook-runnable
//  Boxの所有権をとって、破棄する関数
fn eat_box_i32(boxed_i32: Box<i32>) {
    println!("Destroying box that contains {}", boxed_i32);
}

// i32を借用する関数
fn borrow_i32(borrowed_i32: &i32) {
    println!("This int is: {}", borrowed_i32);
}

fn main() {
    // ヒープ上のi32とスタック上のi32を作る
    let boxed_i32 = Box::new(5_i32);
    let stacked_i32 = 6_i32;

    // Boxの要素を借用する。所有権が取られないため、
    // もう1度借用できる。
    borrow_i32(&boxed_i32);
    borrow_i32(&stacked_i32);

    {
        // Box上のデータの参照を作る
        let _ref_to_i32: &i32 = &boxed_i32;

        // エラー!
        // `boxed_i32`は後に借用されるため、破棄できません。
        eat_box_i32(boxed_i32);
        // FIXME ^ この行をコメントアウトしてください

        // 中の値が破棄された後に`_ref_to_i32`を使用しようとする
        borrow_i32(_ref_to_i32);
        // `_ref_to_i32`がスコープを出たため、もう借用されることはない
    }

    // 今は`boxed_i32`は`eat_box`に所有権を与えて破棄することができる。
    eat_box_i32(boxed_i32);
}
```
