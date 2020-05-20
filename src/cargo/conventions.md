# 慣習

前の節で、このようなディレクトリ階層を見ました。

```txt
foo
├── Cargo.toml
└── src
    └── main.rs
```

同じプロジェクト内でバイナリを2つ以上作るときはどうするのでしょうか?

`cargo`はこれについてもサポートしています。前に見たように`main`がデフォルトのバイナリですが、
`bin/`ディレクトリ内で他のバイナリを作ることができます。

```txt
foo
├── Cargo.toml
└── src
    ├── main.rs
    └── bin
        └── my_other_bin.rs
```

そのバイナリをコンパイル&実行したいときは、`cargo`にバイナリ名を`my_other_bin`として
`--bin my_other_bin`フラグを渡せばよいだけです。

複数のバイナリに加えて、`cargo`は、ベンチマーク、テスト、サンプルなど[多くの機能][more features]
を備えています。

次の節で、テストについて見ていきます。

[more features]: https://doc.rust-lang.org/cargo/guide/project-layout.html
