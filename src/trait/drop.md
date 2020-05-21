# Drop

[`Drop`][Drop]トレイトは唯一つのメソッド`drop`を持ち、オブジェクトがスコープを抜けたときに
自動的に呼び出されます。`Drop`トレイトは主に実装者のインスタンスを開放するのに使われます。

`Box`、`Vec`、`String`、`File`、`Process` などはリソースを開放するために
`Drop`トレイトを実装している型の例です。`Drop`はカスタム型に手動で実装することもできます。

以下の例では、`drop`が呼び出されたときにコンソールに出力します。

```rust,editable
struct Droppable {
    name: &'static str,
}

// `drop`が呼び出されたときにコンソールに出力する小さな実装
impl Drop for Droppable {
    fn drop(&mut self) {
        println!("> Dropping {}", self.name);
    }
}

fn main() {
    let _a = Droppable { name: "a" };

    // ブロックA
    {
        let _b = Droppable { name: "b" };

        // ブロックB
        {
            let _c = Droppable { name: "c" };
            let _d = Droppable { name: "d" };

            println!("Exiting block B");
        }
        println!("Just exited block B");

        println!("Exiting block A");
    }
    println!("Just exited block A");

    // `drop`関数を使って手動で呼び出すこともできます。
    drop(_a);
    // TODO ^ この行をコメントアウトしてください。

    println!("end of the main function");

    // `_a`はもう手動で`drop`されたので、ここでは`drop`されません。
}
```

[Drop]: https://doc.rust-lang.org/std/ops/trait.Drop.html
