# 引数解析

シンプルな引数解析にはmacthを使います。

```rust,editable
use std::env;

fn increase(number: i32) {
    println!("{}", number + 1);
}

fn decrease(number: i32) {
    println!("{}", number - 1);
}

// 使い方:
// match_args <string>
//     与えられた文字列が正しい答えかどうか確認します。
// match_args {{increase|decrease}} <integer>
//     与えられた整数をインクリメントまたはデクリメントします。

fn help() {
    println!("usage:
match_args <string>
    Check whether given string is the answer.
match_args {{increase|decrease}} <integer>
    Increase or decrease given integer by one.");
}

fn main() {
    let args: Vec<String> = env::args().collect();

    match args.len() {
        // 引数が渡されなかった場合
        1 => {
            println!("My name is 'match_args'. Try passing some arguments!");  // 私は"match_args"です。なにか引数を渡してみてください!
        },
        // 1つ引数が渡された場合
        2 => {
            match args[1].parse() {
                Ok(42) => println!("This is the answer!"),
                _ => println!("This is not the answer."),
            }
        },
        // 1つのコマンドと1つの引数が渡された場合
        3 => {
            let cmd = &args[1];
            let num = &args[2];
            // 数値を解析する
            let number: i32 = match num.parse() {
                Ok(n) => {
                    n
                },
                Err(_) => {
                    eprintln!("error: second argument not an integer");
                    help();
                    return;
                },
            };
            // コマンドを解析する
            match &cmd[..] {
                "increase" => increase(number),
                "decrease" => decrease(number),
                _ => {
                    eprintln!("error: invalid command");
                    help();
                },
            }
        },
        // その他の場合
        _ => {
            // show a help message
            help();
        }
    }
}
```

```shell
$ ./match_args Rust
This is not the answer.
$ ./match_args 42
This is the answer!
$ ./match_args do something
error: second argument not an integer
usage:
match_args <string>
    Check whether given string is the answer.
match_args {increase|decrease} <integer>
    Increase or decrease given integer by one.
$ ./match_args do 42
error: invalid command
usage:
match_args <string>
    Check whether given string is the answer.
match_args {increase|decrease} <integer>
    Increase or decrease given integer by one.
$ ./match_args increase 42
43
```
