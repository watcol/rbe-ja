# 整合性テスト

[ユニットテスト][unit]は独立した一つのモジュール上で実行されます。これはプライベート
なコードをテストするには必要です。整合性テストはクレートの外にあり、他のコードがそうで
あるように、パブリックなインターフェースだけにアクセスできます。これはライブラリの
いろいろな部分が適切に動くかテストするのに使います。

Cargoは整合性テストを`src`の隣の`tests`ディレクトリに置くことを想定しています。

ファイル`src/lib.rs`:

```rust,ignore
// このクレートadderは呼び出されることを想定しているため、整合性テストで
// 正しく動くか確認する。
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

テスト用ファイル`tests/integration_test.rs`:

```rust,ignore
// 他のコードがするように、テストするクレートを持ってくる
extern crate adder;

#[test]
fn test_add() {
    assert_eq!(adder::add(3, 2), 5);
}
```

テストを`cargo test`コマンドで実行する

```shell
$ cargo test
running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/integration_test-bcd60824f5fbfe19

running 1 test
test test_add ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests adder

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

`tests`ディレクトリ内の各コードは異なるクレートとしてコンパイルされます。整合性
テスト間でコードを共有したいときはパブリック関数を持つモジュールを定義し、それを
インポートします。

ファイル`tests/common.rs`:

```rust,ignore
pub fn setup() {
    // 必要なファイルやディレクトリを作る、サーバーを起動する
    // などの必要なセットアップを記述する。
}
```

テストファイル`tests/integration_test.rs`

```rust,ignore
// 他のコードがするように、テストするクレートを持ってくる
extern crate adder;

// commonモジュールをインポートする。
mod common;

#[test]
fn test_add() {
    // common内のコードを実行する
    common::setup();
    assert_eq!(adder::add(3, 2), 5);
}
```

このようなコードのモジュール階層は普通の[モジュール][mod]の規則と同じであるため、
`tests/common/mod.rs`のような場所に作ってもOKです。

[unit]: unit_testing.md
[mod]: ../mod.md
