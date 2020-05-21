# メソッド

メソッドは関数と同じようにアノテーションできます。

```rust,editable
struct Owner(i32);

impl Owner {
    // 普通の関数と同じようにライフタイムを明示する
    fn add_one<'a>(&'a mut self) { self.0 += 1; }
    fn print<'a>(&'a self) {
        println!("`print`: {}", self.0);
    }
}

fn main() {
    let mut owner = Owner(18);

    owner.add_one();
    owner.print();
}
```

### こちらも参照:

- [メソッド][methods]

[methods]: ../../fn/methods.md
