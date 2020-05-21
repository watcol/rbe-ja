# トレイト

トレイトでのライフタイムも基本的に関数と同じです。
`impl`も明示的アノテーションを持てることに注意してください。

```rust,editable
// ライフタイムのアノテーションを持つ構造体
#[derive(Debug)]
 struct Borrowed<'a> {
     x: &'a i32,
 }

// implブロックに対してライフタイムを明示する。
impl<'a> Default for Borrowed<'a> {
    fn default() -> Self {
        Self {
            x: &10,
        }
    }
}

fn main() {
    let b: Borrowed = Default::default();
    println!("b is {:?}", b);
}
```

### こちらも参照:

- [`trait`][trait]


[trait]: ../../trait.md
