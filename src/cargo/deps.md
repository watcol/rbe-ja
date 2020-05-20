# 依存

ほとんどのプログラムはライブラリへの依存を持ちます。もし手動で依存を管理したことがないなら、
その苦痛はわからないでしょう。幸運なことに、Rustのエコシステムは`cargo`で標準化されています!
`cargo`でプロジェクトの依存を管理できます。

新しいRustプロジェクトを作る:

```sh
# バイナリ
cargo new foo

# ライブラリ
cargo new --lib foo
```

この章では、バイナリを作ることを想定しています。しかし、コンセプト
はすべて同じです。

上のコマンドを入力すると、次のようなファイル階層になるでしょう:

```txt
foo
├── Cargo.toml
└── src
    └── main.rs
```

`main.rs`はソースファイルの根幹です。何も新しいことはありません。
`Cargo.toml`はこのプロジェクト(`foo`)用の`cargo`設定ファイルです。
中身はこのようになっているでしょう:

```toml
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
```

`[package]`下の`name`フィールドはプロジェクト名です。これはクレートを
`crates.io`に公開するときに使われます。(詳しくは後で) これはコンパイルされた
バイナリの名前にもなります。

`version`フィールドは[セマンティックバージョニング](http://semver.org/)
をベースとしたバージョン番号です。

`authors`フィールドはクレートを公開するときの開発者のリストです。

`[dependencies]`節にプロジェクトの依存を記述します。

例えば、素晴らしいCLIツールを作りたいときは、[crates.io](https://crates.io) (Rust
の公式パッケージレジストリ)にたくさんの素晴らしいクレートがあります。ポピュラーなものでは
[clap](https://crates.io/crates/clap)などが良いでしょう。
これを書いている時点で、`clap`の最新版は`2.27.1`です。 プログラムに依存を追加するには、
`Cargo.toml`の`[dependencies]`節に、`clap = "2.27.1"`という行を加え、もちろん`main.rs`に
`extern crate clap`を加える必要がありますが、それだけで良いです! `clap`をプログラム内で
使えるようになります。

`cargo`は[他のタイプの依存][dependencies]もサポートしています。
こちらが小さなサンプルです。

```toml
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
clap = "2.27.1" # crates.ioから
rand = { git = "https://github.com/rust-lang-nursery/rand" } # オンラインリポジトリから
bar = { path = "../bar" } # ローカルファイルシステムから
```

`cargo`は依存マネージャ以外の機能も備えています。`Cargo.toml`のすべての
設定オプションの一覧はは[format specification][manifest]にあります。

プロジェクトを実行ファイルにビルドするには`cargo build`をプロジェクトディレクトリ内の
いずれかのディレクトリ(サブディレクトリでもOKです!)から実行してください。また、
`cargo run`でビルド後に実行できます。これらのコマンドは、依存関係を解決し、
必要に応じてクレートをダウンロードし、すべてをビルドします。(`make`のように、まだビルド
されていないもののみビルドします。)

たったこれだけです!


[manifest]: https://doc.rust-lang.org/cargo/reference/manifest.html
[dependencies]: https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html
