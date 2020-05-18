# 発散関数

発散関数は値を返しません。空の型である`!`を使って示してください。

```rust
fn foo() -> ! {
    panic!("This call never returns.");  // この呼び出しは決して返りません。
}
```

他の方とは違い、この型はインスタンス化できません。この型には持ちうる値
がないからです。一つの値を持ちうる`()`型とは違うことに注意してください。

例えば、この関数は、返り値の型に関する情報がありませんが、
普通に値を返します。

```rust
fn some_fn() {
    ()
}

fn main() {
    let a: () = some_fn();
    println!("This function returns and you can see this line.")  // この関数は値を返し、この行が出力されます。
}
```

この関数とは違い、呼び出し元に制御が戻ることはありません。

```rust,ignore
#![feature(never_type)]

fn main() {
    let x: ! = panic!("This call never returns.");
    println!("You will never see this line!");  // この行は出力されません。
}
```

これは、抽象的な概念に見えますが、実はとても有用で、手頃です。
この型の主な利点は、どんな型にもキャストでき、`match`の枝など、
正確な型が必要なときに使えます。これを使って、このような関数が書けます。

```rust
fn main() {
    fn sum_odd_numbers(up_to: u32) -> u32 {
        let mut acc = 0;
        for i in 0..up_to {
            // このmatch式は、変数additionの型であるu32を返さなければ
            // いけないことに注意してください。
            let addition: u32 = match i%2 == 1 {
                // 変数iは u32になるので、問題ありません。
                true => i,
                // 一方、"continue"式はu32を返しませんが、何も返さないことは、
                // match式の型の要件に違反しないので、これでも問題ありません。
                false => continue,
            };
            acc += addition;
        }
        acc
    }
    println!("Sum of odd numbers up to 9 (excluding): {}", sum_odd_numbers(9));  // 9未満の奇数の和: {}
}
```

ネットワークサーバのように、永遠にループ(例えば`loop {}`)する処理や、
プロセスを終了する関数(例えば`exit()`)などに使われます。
