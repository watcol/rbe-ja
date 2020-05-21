# トレイト

トレイトは、未知の型`Self`に対して実装されたメソッドの集合です。
ここでは、同じトレイトで実装されたメソッドにアクセスすることができます。

トレイトはいかなるデータ型にも実装できます。下の例では、メソッド群`Animal`
を定義し、`Animal`トレイトをデータ型`Sheep`に対して実装することで、`Animal`
のメソッドを`Sheep`から使えるようにしています。

```rust,editable
struct Sheep { naked: bool, name: &'static str }

trait Animal {
    //  コンストラクタとなる静的メソッドを定義する。`Self`は実装する型を表す。
    fn new(name: &'static str) -> Self;

    // インスタンスメソッドの定義。ここでは文字列を返す。
    fn name(&self) -> &'static str;
    fn noise(&self) -> &'static str;

    // トレイトはデフォルト実装を提供することができる。
    fn talk(&self) {
        println!("{} says {}", self.name(), self.noise());
    }
}

impl Sheep {
    fn is_naked(&self) -> bool {
        self.naked
    }

    fn shear(&mut self) {
        if self.is_naked() {
            // 実装者のメソッドは実装者のトレイトで定義したメソッドを使うことができる。
            println!("{} is already naked...", self.name());
        } else {
            println!("{} gets a haircut!", self.name);

            self.naked = true;
        }
    }
}

// `Animal`トレイトを`Sheep`に実装する。
impl Animal for Sheep {
    // `Self`は実装する型(`Sheep`)
    fn new(name: &'static str) -> Sheep {
        Sheep { name: name, naked: false }
    }

    fn name(&self) -> &'static str {
        self.name
    }

    fn noise(&self) -> &'static str {
        if self.is_naked() {
            "baaaaah?"
        } else {
            "baaaaah!"
        }
    }
    
    // デフォルトメソッドは上書きできる。
    fn talk(&self) {
        // 例えば、熟考を加えることができる。
        println!("{} pauses briefly... {}", self.name, self.noise());  // {}は一息おいた... {}
    }
}

fn main() {
    // この場合、型注釈は必要です。
    let mut dolly: Sheep = Animal::new("Dolly");
    // TODO ^ 型注釈を消してみてください。

    dolly.talk();
    dolly.shear();
    dolly.talk();
}
```
