# 開発時依存

時々、テストにのみ必要な依存がある場合があります(例えばベンチマーク)
このような依存は`Cargo.toml`の`[dev-dependencies]`節に追加してください。
この依存は、このクレートに依存する他のクレートには影響しません。

`assert!`マクロを拡張するクレートを例に取ってみましょう。
ファイル`Cargo.toml`:

```toml
# いつものデータは省略します
[dev-dependencies]
pretty_assertions = "0.4.0"
```

ファイル`src/lib.rs`:

```rust,ignore
// クレートをテストのみの使用に制限する
#[cfg(test)]
#[macro_use]
extern crate pretty_assertions;

pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5);
    }
}
```

## こちらも参照
[Cargo][cargo]のドキュメントの「依存を指定する」章

[cargo]: http://doc.crates.io/specifying-dependencies.html
