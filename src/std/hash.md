# ハッシュマップ

ベクターは整数でインデックスを管理しましたが、`HashMap`はキーで値を保持します。
`HashMap`キーは、真偽値、整数、文字列など、`Eq`トレイトと`Hash`トレイトを実装する
すべての型をキーにできます。これについては次の節で詳しく扱います。

ベクターのように、`HashMap`は拡張でき、さらに余分なスペースを縮小できます。
容量を指定して`HashMap::with_capacity(uint)`とするか、`HashMap::new()`で
デフォルト容量でHashMapを初期化する(推奨)ことで、HashMapを作ることができます。

```rust,editable
use std::collections::HashMap;

fn call(number: &str) -> &str {
    match number {
        "798-1364" => "We're sorry, the call cannot be completed as dialed.  
            Please hang up and try again.",  // ただいま電話に出ることができません。しばらくしてもう一度おかけください。
        "645-7689" => "Hello, this is Mr. Awesome's Pizza. My name is Fred.
            What can I get for you today?",  // こんにちは。Mr. Awesome's PizzaのFredです。何をご注文されますか?
        _ => "Hi! Who is this again?"  // もしもし、誰ですか?
    }
}

fn main() { 
    let mut contacts = HashMap::new();

    contacts.insert("Daniel", "798-1364");
    contacts.insert("Ashley", "645-7689");
    contacts.insert("Katie", "435-8291");
    contacts.insert("Robert", "956-1745");

    // 参照をとり、Option<&V>を返す
    match contacts.get(&"Daniel") {
        Some(&number) => println!("Calling Daniel: {}", call(number)),
        _ => println!("Don't have Daniel's number."),  // Danielの番号を持っていません
    }

    // `HashMap::insert()`は`None`を返す
    // if the inserted value is new, `Some(value)` otherwise
    contacts.insert("Daniel", "164-6743");

    match contacts.get(&"Ashley") {
        Some(&number) => println!("Calling Ashley: {}", call(number)),
        _ => println!("Don't have Ashley's number."),
    }

    contacts.remove(&"Ashley"); 

    // `HashMap::iter()`はイテレータを返します。
    // (&'a key, &'a value)は順番になる。
    for (contact, &number) in contacts.iter() {
        println!("Calling {}: {}", contact, call(number)); 
    }
}
```

ハッシュやハッシュマップ(ハッシュテーブルとも呼ばれます)
についての詳しい情報は、[Hash Table Wikipedia][wiki-hash]
を参照してください。

[wiki-hash]: https://en.wikipedia.org/wiki/Hash_table
