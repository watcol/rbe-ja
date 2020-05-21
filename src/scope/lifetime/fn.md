# 関数

[省略][elision]しなかった場合、関数のライフタイムにはいくつかの制限があります。

* すべての参照は明示的なライフタイムを持つ必要がある。
* 返り値となる参照は、入力と同じか、`'static`ライフタイムを持つ必要がある。

更に、返り値となる参照は、入力がなけく、無効なデータへの参照だった場合エラーになります。
以下の例ではライフタイムを使ったいくつかの有効な関数を示しています。

```rust,editable
// 少なくともこの関数より長いライフタイム`'a`を持つ
// 一つの参照を入力として受け取る関数
fn print_one<'a>(x: &'a i32) {
    println!("`print_one`: x is {}", x);
}

// ライフタイムが合っても可変参照は使えます。
fn add_one<'a>(x: &'a mut i32) {
    *x += 1;
}

// 異なるライフタイムを持つ複数の要素。ここでは、同じライフタイム`'a`
// にしても良いかもしれませんが、もっと複雑なケースでは異なるライフタイム
// が必要です。
fn print_multi<'a, 'b>(x: &'a i32, y: &'b i32) {
    println!("`print_multi`: x is {}, y is {}", x, y);
}

// 渡された変数はそのまま返すことができます。
// しかし、正しいライフタイムが返される必要があります。
fn pass_x<'a, 'b>(x: &'a i32, _: &'b i32) -> &'a i32 { x }

//fn invalid_output<'a>() -> &'a String { &String::from("foo") }
// 上は無効です。`'a`は関数より長いライフタイムを持つ必要がありますが、
// ここで、`&String::from("foo")`は`String`を作り、その参照を返します。
// スコープを抜けたときにデータがdropされるため、無効なデータへの参照を
// 返すことになります。

fn main() {
    let x = 7;
    let y = 9;
    
    print_one(&x);
    print_multi(&x, &y);
    
    let z = pass_x(&x, &y);
    print_one(z);

    let mut t = 3;
    add_one(&mut t);
    print_one(&t);
}
```

### こちらも参照:

- [functions][fn]

[elision]: elision.md
[fn]: fn.md
