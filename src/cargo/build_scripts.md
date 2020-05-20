# ビルドスクリプト

時々、デフォルトの`cargo`のビルドでは不十分な場合があります。おそらくそれは`cargo`が
コンパイルを成功するのにコード生成、他のファイルのコンパイルなどを事前にしなければ
いけないときでしょう。この問題を解決するために、Cargoではビルドスクリプトが実行できます。

`Cargo.toml`に以下のような行を加えると、ビルドスクリプトが追加できます。

```toml
[package]
...
build = "build.rs"
```

デフォルトでCargoはプロジェクトディレクトリの`build.rs`ファイルがあれば、
実行します。

## ビルドスクリプトの使い方

ビルドスクリプトは単純にはじめにコンパイルされ、パッケージ内の他のものをコンパイルする
Rustファイルです。これによってクレートの事前要件を満たすことができます。

Cargoは[ここで指定された][specified here]環境変数を入力として実行されるスクリプトをいくつか提供しています。

また、スクリプトは標準出力を介して出力し、`target/debug/build/<pkg>/output`にすべての
出力が書き込まれます。さらに、`cargo:`で始まる行はCargoに解析され、パッケージをコンパイル
するときのひきすうとして使われます。

さらなる仕様やサンプルについては[Cargoのドキュメント][cargo_specification]を参照してください。

[specified here]: https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-build-scripts

[cargo_specification]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
