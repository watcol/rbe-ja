# ループの返り値

`loop`の使い道の一つは成功するまでオペレーションを繰り返すことです。オペレーション
が値を返す時、それを残りのコードに渡す必要があるかもしれません。
それを`break`に渡せば、`loop`式から値が返されます。

```rust,editable
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    assert_eq!(result, 20);
}
```
