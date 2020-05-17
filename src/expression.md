# 式

Rustプログラムは(ほとんど)文の集合でできています。

```
fn main() {
    // 文
    // 文
    // 文
}
```

Rustの文にはいくつかの種類があります。最も一般的な2つは変数束縛と、
`;`を使った式です。

```
fn main() {
    // 変数束縛
    let x = 5;

    // 式;
    x;
    x + 1;
    15;
}
```

ブロックも式です。なので、値として扱うことができます。その場合、
ブロックの最後の式がローカル変数のような場所を表す式に代入されます。
ただし、ブロックの最後の式がセミコロンで終わる場合は、返り値は`()`に
なります。

```rust,editable
fn main() {
    let x = 5u32;

    let y = {
        let x_squared = x * x;
        let x_cube = x_squared * x;

        // This expression will be assigned to `y`
        x_cube + x_squared + x
    };

    let z = {
        // セミコロンがあるので、`z`には`()`が代入されます。
        2 * x;
    };

    println!("x is {:?}", x);
    println!("y is {:?}", y);
    println!("z is {:?}", z);
}
```
