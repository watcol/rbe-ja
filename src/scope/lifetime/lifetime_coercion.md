# ライフタイムの圧縮

自分より短いライフタイムを持つものに自分を圧縮し、そのままでは
動かないスコープの中でも使えるようにすることができます。これには、
コンパイラが推論して圧縮する場合と、ライフタイムの違いを見て圧縮する
場合があります。

```rust,editable
// ここで、Rustはできるだけ短いライフタイムを推論し、
// 2つの参照をそれに圧縮します。
fn multiply<'a>(first: &'a i32, second: &'a i32) -> i32 {
    first * second
}

// `<'a: 'b, 'b>`は、`'a`は少なくとも`'b`より長いことを意味します。
// ここでは、`&'a i32`を`&'b i32`に圧縮して返す。
fn choose_first<'a: 'b, 'b>(first: &'a i32, _: &'b i32) -> &'b i32 {
    first
}

fn main() {
    let first = 2; // 長いライフタイム
    
    {
        let second = 3; // 短いライフタイム
        
        println!("The product is {}", multiply(&first, &second));
        println!("{} is the first", choose_first(&first, &second));
    };
}
```
