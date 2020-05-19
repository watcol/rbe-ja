# `use`宣言

`use`宣言は、アクセスを簡単にするため、パスに新しい名前を束縛します。
このように使います。

```rust,editable,ignore
// extern crate deeply; // 普通、この行はコメントアウトされていません!

use crate::deeply::nested::{
    my_first_function,
    my_second_function,
    AndATraitType
};

fn main() {
    my_first_function();
}
```

違う名前に束縛してインポートするのに`as`キーワードを使います。

```rust,editable
// `deeply::nested::function`を`other_function`に束縛する。
use deeply::nested::function as other_function;

fn function() {
    println!("called `function()`");
}

mod deeply {
    pub mod nested {
        pub fn function() {
            println!("called `deeply::nested::function()`");
        }
    }
}

fn main() {
    // 簡単に`deeply::nested::function`にアクセスできる。
    other_function();

    println!("Entering block");
    {
        // これは`use deeply::nested::function as function`と等価で、
        // `function()`はもともとのものを覆い隠します。
        use crate::deeply::nested::function;
        function();

        // `use`束縛はローカルスコープを持っています。ここでは、
        // `function()`のシャドーイングはこのブロック内だけで有効です。
        println!("Leaving block");
    }

    function();
}
```
