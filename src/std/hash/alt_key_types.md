# 独自のキーの型

`Eq`トレイトと`Hash`トレイトを実装しているすべての型は`HashMap`の
キーとして使用できます。

* `bool` (値を2つしか保持できないので使えないと思います)
* `int`、`uint`などのすべての整数型
* `String`や`&str` (`String`をキーとする`HashMap`で`.get()`を
呼び出すと`&str`が帰ってきます。

`f32`と`f64`は[浮動小数点の精度エラー][floating]
が出る可能性があるため、`Hash`を実装していません。
そのため、hashmapには使用できません。

すべてのコレクションは`Eq`と`Hash`を実装している場合があります、例えば、
`T`が`Hash`を実装している時、`Vec<T>`にも`Hash`が実装されます。

この一行で簡単にカスタム型に`Eq`と`Hash`を実装することができます。
`#[derive(PartialEq, Eq, Hash)]`

後はコンパイラが勝手にやってくれます。もっと詳細に制御したい場合は、
自分で`Eq`や`Hash`を実装することもできます。このガイドでは`Hash`の
実装方法については触れません。

`struct`を`HashMap`に対して使い、シンプルなユーザーログオン
システムを作ってみましょう。

```rust,editable
use std::collections::HashMap;

// Eqを継承するには、PartialEqが実装されている必要があります。
#[derive(PartialEq, Eq, Hash)]
struct Account<'a>{
    username: &'a str,
    password: &'a str,
}

struct AccountInfo<'a>{
    name: &'a str,
    email: &'a str,
}

type Accounts<'a> = HashMap<Account<'a>, AccountInfo<'a>>;

fn try_logon<'a>(accounts: &Accounts<'a>,
        username: &'a str, password: &'a str){
    println!("Username: {}", username);
    println!("Password: {}", password);
    println!("Attempting logon...");

    let logon = Account {
        username,
        password,
    };

    match accounts.get(&logon) {
        Some(account_info) => {
            println!("Successful logon!");
            println!("Name: {}", account_info.name);
            println!("Email: {}", account_info.email);
        },
        _ => println!("Login failed!"),
    }
}

fn main(){
    let mut accounts: Accounts = HashMap::new();

    let account = Account {
        username: "j.everyman",
        password: "password123",
    };

    let account_info = AccountInfo {
        name: "John Everyman",
        email: "j.everyman@email.com",
    };

    accounts.insert(account, account_info);

    try_logon(&accounts, "j.everyman", "psasword123");

    try_logon(&accounts, "j.everyman", "password123");
}
```

[hash]: https://en.wikipedia.org/wiki/Hash_function
[floating]: https://en.wikipedia.org/wiki/Floating_point#Accuracy_problems
