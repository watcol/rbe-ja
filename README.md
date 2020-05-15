# rbe-ja
[Rust by Example](https://doc.rust-lang.org/rust-by-example/)の日本語訳です。
このリポジトリは[こちら](https://github.com/rust-lang/rust-by-example)の
2020/05/15頃のソースをベースとしていますが、Github Pagesに関して問題が起きたため、
Forkはしていません。また、不要なファイルは除いています。


## ビルド
[mdBook](https://rust-lang.github.io/mdBook/)をインストールして、以下のコマンドを叩いてください。
```bash
$ mdbook build -d docs
```
この後, `docs/index.html`に好きなブラウザからアクセスすると、ローカルから閲覧できます。
(インターネットに接続していれば、ちゃんとオンラインエディタもライブコードも動きます。)

また、Github Pagesをこのリポジトリから使用している都合上、ビルド結果もリポジトリに残しています。
