# `super`と`self`

`super`、`self`キーワードはパス内での要素へのアクセスで曖昧さをなくし、
パスの不要なハードコードを防ぐのに使われます。

```rust,editable
fn function() {
    println!("called `function()`");
}

mod cool {
    pub fn function() {
        println!("called `cool::function()`");
    }
}

mod my {
    fn function() {
        println!("called `my::function()`");
    }
    
    mod cool {
        pub fn function() {
            println!("called `my::cool::function()`");
        }
    }
    
    pub fn indirect_call() {
        // このスコープから`function`という名前のすべての関数にアクセスしましょう!
        print!("called `my::indirect_call()`, that\n> ");
        
        // `self`キーワードは現在のモジュール(ここでは`my`)を参照します。
        // `self::function()`と`function()`は等価で、同じ結果を返します。 
        self::function();
        function();
        
        // `self`で`my`内の他のモジュールにもアクセスできます。
        self::cool::function();
        
        // `super`キーワードで親スコープ(ここでは`my`モジュールの外側)にアクセスできます。
        super::function();
        
        // ここで*crate*スコープの`cool::function`を束縛します。
        // ここでcrateスコープとは一番外側のスコープのことです。
        {
            use crate::cool::function as root_function;
            root_function();
        }
    }
}

fn main() {
    my::indirect_call();
}
```
