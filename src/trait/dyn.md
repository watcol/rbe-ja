# `dyn`でトレイトを返す

Rustコンパイラは返り値にどのくらいスペースが必要か事前に把握する必要があります。これは、すべての関数が具象型を返す必要があることを意味します。他の言語と違って、
異なる実装は異なるメモリ容量を持つため、`Animal`のようなトレイトを持っていたとしても、`Animal`を返す関数は書けません。

しかし、これには簡単な回避策があります。トレイとオブジェクトを直接返す代わりに、`Animal`を含む`Box`型を返せばよいのです。`box`はヒープメモリの参照に
過ぎませんが、参照は固定されたサイズを持っている上、コンパイラはヒープに格納された`Animal`の参照を返すことが保証されているため、関数からトレイトを返すことが
できます!

Rustはヒープへのメモリ確保ができるだけ明示的になるようにしてきました。そのため、ヒープに確保されたトレイトの参照を返すときも、
返す型を`dyn`キーワードで明示しなければいけません (例えば`Box<dyn Animal>`)

```rust,editable
struct Sheep {}
struct Cow {}

trait Animal {
    // インスタンスメソッド
    fn noise(&self) -> &'static str;
}

// `Sheep`に`Animal`トレイトを実装する。
impl Animal for Sheep {
    fn noise(&self) -> &'static str {
        "baaaaah!"
    }
}

// `Cow`に`Animal`トレイトを実装する。
impl Animal for Cow {
    fn noise(&self) -> &'static str {
        "moooooo!"
    }
}

// Animalを実装したいくつかの構造体を返しますが、コンパイル時にはその型がわかりません。
fn random_animal(random_number: f64) -> Box<dyn Animal> {
    if random_number < 0.5 {
        Box::new(Sheep {})
    } else {
        Box::new(Cow {})
    }
}

fn main() {
    let random_number = 0.234;
    let animal = random_animal(random_number);
    println!("You've randomly chosen an animal, and it says {}", animal.noise());
}

```
