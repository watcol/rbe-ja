# `Rc`

複数のの所有権が必要な場合、`Rc`(Reference Counting、参照カウント)が使えます。`Rc`は内部に保持している値がいくつの場所から使用されているのかを記録しています。 

`Rc`が複製された時カウントが1増え、`Rc`がスコープの外にdropされた時、カウントが1減ります。
カウントが0になった時、所有権は残っていないことを意味するため、値が開放されます。

`Rc`を複製してもコピーは作りません。複製したらもう一つのポインタを作り、カウントを増やすだけです。

```rust,editable
use std::rc::Rc;

fn main() {
    let rc_examples = "Rc examples".to_string();
    {
        println!("--- rc_a is created ---");
        
        let rc_a: Rc<String> = Rc::new(rc_examples);
        println!("Reference Count of rc_a: {}", Rc::strong_count(&rc_a));
        
        {
            println!("--- rc_a is cloned to rc_b ---");
            
            let rc_b: Rc<String> = Rc::clone(&rc_a);
            println!("Reference Count of rc_b: {}", Rc::strong_count(&rc_b));
            println!("Reference Count of rc_a: {}", Rc::strong_count(&rc_a));
            
            // `Rc`中の値が等しい時、Rcは等しいです。
            println!("rc_a and rc_b are equal: {}", rc_a.eq(&rc_b));
            
            // 値のメソッドを直接呼び出すことができます。
            println!("Length of the value inside rc_a: {}", rc_a.len());
            println!("Value of rc_b: {}", rc_b);
            
            println!("--- rc_b is dropped out of scope ---");
        }
        
        println!("Reference Count of rc_a: {}", Rc::strong_count(&rc_a));
        
        println!("--- rc_a is dropped out of scope ---");
    }
    
    // エラー! `rc_examples`はすでに`rc_a`にムーブされています。
    // `rc_a`がドロップされた時、`rc_examples`もドロップされます。
    // println!("rc_examples: {}", rc_examples);
    // TODO ^ この行をアンコメントしてみてください。
}
```

### こちらも参照:

- [std::rc][1]
- [Arc][2]

[1]: https://doc.rust-lang.org/std/rc/index.html
[2]: https://doc.rust-lang.org/std/sync/struct.Arc.html
