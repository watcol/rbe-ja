# 可視性

デフォルトで、モジュールの要素はプライベートです。しかし、`pub`修飾子でこれを
上書きすることができます。パブリックな要素のみがモジュールスコープの外からアクセス
できます。

```rust,editable
// `my_mod`というモジュールを定義する
mod my_mod {
    // モジュールの要素はデフォルトでプライベートです。
    fn private_function() {
        println!("called `my_mod::private_function()`");
    }

    // `pub`修飾子を使って上書きできます。
    pub fn function() {
        println!("called `my_mod::function()`");
    }

    // モジュール内では、プライベートな要素を含むすべての
    // 他の要素にアクセスできます。
    pub fn indirect_access() {
        print!("called `my_mod::indirect_access()`, that\n> ");
        private_function();
    }

    // モジュールはネストできます
    pub mod nested {
        pub fn function() {
            println!("called `my_mod::nested::function()`");
        }

        #[allow(dead_code)]
        fn private_function() {
            println!("called `my_mod::nested::private_function()`");
        }

        // 関数を`pub(in パス)`構文を使って宣言すると、指定したパスからのみ
        // アクセスできます。`path`は自分の親か、先祖のモジュールでなければいけません。
        pub(in crate::my_mod) fn public_function_in_my_mod() {
            print!("called `my_mod::nested::public_function_in_my_mod()`, that\n> ");
            public_function_in_nested();
        }

        // 関数を`pub(self)`構文を使って宣言すると、現在のモジュールでのみ
        // アクセスでき、プライベートと同じになります。
        pub(self) fn public_function_in_nested() {
            println!("called `my_mod::nested::public_function_in_nested()`");
        }

        // 関数を`pub(super)`を構文を使って宣言すると、親のモジュールからのみ
        // アクセスできます。
        pub(super) fn public_function_in_super_mod() {
            println!("called `my_mod::nested::public_function_in_super_mod()`");
        }
    }

    pub fn call_public_function_in_my_mod() {
        print!("called `my_mod::call_public_function_in_my_mod()`, that\n> ");
        nested::public_function_in_my_mod();
        print!("> ");
        nested::public_function_in_super_mod();
    }

    // pub(crate)で関数が現在のクレート内でアクセスできるようになります。
    pub(crate) fn public_function_in_crate() {
        println!("called `my_mod::public_function_in_crate()`");
    }

    // ネストされたモジュールも同じ規則に従います。
    mod private_nested {
        #[allow(dead_code)]
        pub fn function() {
            println!("called `my_mod::private_nested::function()`");
        }

        // 親モジュールのプライベートな要素にもアクセスできます。
        // 先祖のモジュールでも同じです。
        #[allow(dead_code)]
        pub(crate) fn restricted_function() {
            println!("called `my_mod::private_nested::restricted_function()`");
        }
    }
}

fn function() {
    println!("called `function()`");
}

fn main() {
    // モジュールで同じ名前をもつ要素の明確化ができます。
    function();
    my_mod::function();

    // ネストされたモジュールも含めて、パブリックな要素には
    // 親モジュールの外からでもアクセスできます。
    my_mod::indirect_access();
    my_mod::nested::function();
    my_mod::call_public_function_in_my_mod();

    // pub(crate)の要素は同じクレート内のどこからでもアクセスできます。
    my_mod::public_function_in_crate();

    // pub(in パス)の要素は指定したモジュールからのみアクセスできます。
    // エラー! `public_function_in_my_mod`関数はプライベートです
    //my_mod::nested::public_function_in_my_mod();
    // TODO ^ この行をアンコメントしてみてください

    // プライベートな要素は、パブリックなモジュールにネスト
    // されていたとしても、直接アクセスできません。

    // エラー! `private_function`はプライベートです。
    //my_mod::private_function();
    // TODO ^ この行をアンコメントしてみてください

    // エラー! `private_function`はプライベートです
    //my_mod::nested::private_function();
    // TODO ^ この行をアンコメントしてみてください

    // エラー! `private_nested`はプライベートモジュールです
    //my_mod::private_nested::function();
    // TODO ^ この行をアンコメントしてみてください

    // エラー! `private_nested`はプライベートモジュールです
    //my_mod::private_nested::restricted_function();
    // TODO ^ この行をアンコメントしてみてください
}
```
