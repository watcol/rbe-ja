# `?`でOptionを解析する

`match`文で`Option`を解析できますが、しばしば`?`演算子を使った方が簡単です。
もし`x`が`Option`だとしたら、`x?`と書けば、`x`が`Some`ならその値を、そうで
なければ現在の関数をNoneを返して終了します。

```rust,editable
fn next_birthday(current_age: Option<u8>) -> Option<String> {
	// `current_age`が`None`なら`None`を返す。
	// `current_age`が`Some`なら、`u8`は`next_age`に代入される
    let next_age: u8 = current_age?;
    Some(format!("Next year I will be {}", next_age))
}
```

コードの可読性をあげるため、`?`はたくさん連結できます。

```rust,editable
struct Person {
    job: Option<Job>,
}

#[derive(Clone, Copy)]
struct Job {
    phone_number: Option<PhoneNumber>,
}

#[derive(Clone, Copy)]
struct PhoneNumber {
    area_code: Option<u8>,
    number: u32,
}

impl Person {

    // その人の仕事用の電話番号のエリアコードを取得します。
    fn work_phone_area_code(&self) -> Option<u8> {
        // `?`演算子がなければ、`match`文をたくさんネストする必要がありました。
        // 簡単に連結を増やすこともできます。試してみてください。
        self.job?.phone_number?.area_code
    }
}

fn main() {
    let p = Person {
        job: Some(Job {
            phone_number: Some(PhoneNumber {
                area_code: Some(61),
                number: 439222222,
            }),
        }),
    };

    assert_eq!(p.work_phone_area_code(), Some(61));
}
```
