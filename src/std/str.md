# 文字列

Rustには2つのタイプの文字列`String`、`&str`があります。

`String`は`Vec<u8>`でバイト列が保存されていますが、UTF-8が使えることは保証されて
います。`String`はヒープに保存されるので、可変長で、ヌル文字で終わりません。

`&str`はスライス`&[u8]`であり、いつも有効なUTF-8文字列を参照しています。
そしてこれは`&[T]`が`Vec<T>`に変換できるように、`String`に変換できます。

```rust,editable
fn main() {
    // (すべての型注釈は必要ないものです)
    // 読み取りメモリに保存された文字列へのポインタ
    let pangram: &'static str = "the quick brown fox jumps over the lazy dog";  // 茶色く素早い狐がのろまな犬を飛び越える
    println!("Pangram: {}", pangram);

    // 単語を逆順でイテレートします。新しくメモリの確保はされません。
    println!("Words in reverse");
    for word in pangram.split_whitespace().rev() {
        println!("> {}", word);
    }

    // 文字をすべてベクターにコピーし、ソートし、重複を消す。
    let mut chars: Vec<char> = pangram.chars().collect();
    chars.sort();
    chars.dedup();

    // 空の可変長な`String`を作る
    let mut string = String::new();
    for c in chars {
        // 文字列の最後に文字を挿入する
        string.push(c);
        // 文字列の最後に文字列を挿入する
        string.push_str(", ");
    }

    // 文字列をトリミングすると、元の文字列のスライスを作るため、
    // 新しくメモリ確保はされない。
    let chars_to_trim: &[char] = &[' ', ','];
    let trimmed_str: &str = string.trim_matches(chars_to_trim);
    println!("Used characters: {}", trimmed_str);

    // 文字列をヒープに格納する。
    let alice = String::from("I like dogs");
    // 新しくメモリ確保して、置換後の文字列を格納する。
    let bob: String = alice.replace("dog", "cat");

    println!("Alice says: {}", alice);
    println!("Bob says: {}", bob);
}
```

`str`と`String`のさらなるメソッドについては[std::str][str]や
[std::string][string]モジュールのドキュメントを参照してください。

## リテラルとエスケープ

文字列のリテラルを書き、特殊文字をその中に入れる方法は複数あり、そのすべてが
、最も書きやすく、扱いやすい`&str`になります。同様にバイト文字列リテラルの書き方
は複数ありますが、すべて`&[u8; N]`になります。

一般的に特殊文字はバックスラッシュ`\`でエスケープできます。これを使えば、
出力できない文字や、打ち方を知らない文字でも表すことができます。バックスラッシュ
のリテラルは`\\`と表します。

文字列や文字のリテラルの終端を表す文字は、`"\""`や`'\''`で表します。

```rust,editable
fn main() {
    // 16進数で文字を一つ一つ書くこともできます...
    let byte_escape = "I'm writing \x52\x75\x73\x74!";
    println!("What are you doing\x3F (\\x3F means ?) {}", byte_escape);

    // ...Unicodeコードポイントも使えます。
    let unicode_codepoint = "\u{211D}";
    let character_name = "\"DOUBLE-STRUCK CAPITAL R\"";

    println!("Unicode character {} (U+211D) is called {}",
                unicode_codepoint, character_name );


    let long_string = "String literals  // 文字列リテラルは改行やインデントもエスケープできます。
                        can span multiple lines.
                        The linebreak and indentation here ->\
                        <- can be escaped too!";
    println!("{}", long_string);
}
```

エスケープに使われる文字をたくさん使った文字列を書きたいときもあります。
この場合はRaw文字列リテラルを使います。

```rust, editable
fn main() {
    let raw_str = r"Escapes don't work here: \x3F \u{211D}";
    println!("{}", raw_str);

    // Raw1文字列にクオートを使うときは、#で囲みます。
    let quotes = r#"And then I said: "There is no escape!""#;
    println!("{}", quotes);

    // "も#も使う場合は、さらに#を重ねてください。
    // #の重ねられる数に上限はありません。
    let longer_delimiter = r###"A string with "# in it. And even "##!"###;
    println!("{}", longer_delimiter);
}
```

UTF-8出ない文字列を使いたいですか? (`str`と`String`はUTF-8でないといけません)
またはバイト列を文字列として使いたいですか? バイト文字列が使えます!

```rust, editable
use std::str;

fn main() {
    // これは`&str`ではありません。
    let bytestring: &[u8; 21] = b"this is a byte string";

    // バイト列は`Display`トレイトを持っていないので、出力は多少難しいです。
    println!("A byte string: {:?}", bytestring);

    // バイト文字列はバイトエスケープができます...
    let escaped = b"\x52\x75\x73\x74 as bytes";
    // ...しかしユニコードエスケープはできません
    // let escaped = b"\u{211D} is not allowed";
    println!("Some escaped bytes: {:?}", escaped);


    // Rawバイト列もRaw文字列のように使えます。
    let raw_bytestring = br"\u{211D} is not escaped here";
    println!("{:?}", raw_bytestring);

    // バイト列から`str`への変換は時々失敗します。
    if let Ok(my_str) = str::from_utf8(raw_bytestring) {
        println!("And the same as text: '{}'", my_str);
    }

    let _quotes = br#"You can also use "fancier" formatting, \
                    like with normal raw strings"#;

    // バイト文字列はUTF-8である必要はありません。
    let shift_jis = b"\x82\xe6\x82\xa8\x82\xb1\x82\xbb"; // Shift-JISで"ようこそ"

    // しかし`str`には変換できません
    match str::from_utf8(shift_jis) {
        Ok(my_str) => println!("Conversion successful: '{}'", my_str),
        Err(e) => println!("Conversion failed: {:?}", e),
    };
}
```

エンコードの違う文字列を変換するには[encoding][encoding-crate]クレートを使用します。

リテラルの書き方やエスケープシーケンスの詳細はRustリファレンスの
['Tokens'][tokens]章を参照してください。

[str]: https://doc.rust-lang.org/std/str/
[string]: https://doc.rust-lang.org/std/string/
[tokens]: https://doc.rust-lang.org/reference/tokens.html
[encoding-crate]: https://crates.io/crates/encoding
