# テスト

あらゆるソフトにとって、テストは重要です! Rustはテストについての
サポートを提供しています。*The Rust Programming Language*の([この章
](https://doc.rust-lang.org/book/ch11-00-testing.html)を参照してください。

上のドキュメントでユニットテストと整合性テストの書き方について述べています。
ユニットテストや整合性テストは`tests/`ディレクトリに入れることになっています。

```txt
foo
├── Cargo.toml
├── src
│   └── main.rs
└── tests
    ├── my_test.rs
    └── my_other_test.rs
```

`tests`内のそれぞれのファイルが別のテストです。

`cargo`はすべてのテストを自動的に実行する機能を提供しています!

```shell
$ cargo test
```

このような出力が得られるはずです:

```shell
$ cargo test
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.89 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 3 tests
test test_bar ... ok
test test_baz ... ok
test test_foo_bar ... ok
test test_foo ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

パターンにマッチするテストのみを実行することもできます:

```shell
$ cargo test test_foo
```

```shell
$ cargo test test_foo
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.35 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 2 tests
test test_foo ... ok
test test_foo_bar ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```

注意として、Cargoは複数のテストを並列的に実行することがあります。そのため、
すべてのテストがお互いに競合しないようにする必要があります。例えば、それぞれの
テストがファイルに出力する時、違うファイル名に出力する必要があります。
