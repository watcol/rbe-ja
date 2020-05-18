# forループ

## forとrange

`for in`文は`イテレータ`を使って繰り返し処理をするのに使います。
イテレータを作る最も簡単な方法の一つは、`a..b`のように書くことです。
これは`a`(含む)から`b`(除く)までの数値を順に産出(`yield`)します。

`while`の代わりに`for`を使ってFizzBuzzを書いてみましょう。

```rust,editable
fn main() {
    // `n`はそれぞれの繰り返しで1, 2, ..., 100の値をとります
    for n in 1..101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

代わりに`a..=b`で両端を含むことができます。
上の例は次のように書けます。

```rust,editable
fn main() {
    // `n`はそれぞれの繰り返しで1, 2, ..., 100の値をとります
    for n in 1..=100 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

## forとイテレータ

`for in`文は`イテレータ`といくつかの方法で相互作用することができます。
[イテレータ][iter]トレイトの節で議論したように、デフォルトで`for`ループは
コレクションの`into_iter`関数を実行しますが、これだけがコレクションを
イテレータに変換するわけではありません。

`into_iter`、`iter`、`iter_mut`はその中のデータに対する異なる見方を使って、
違った方法でコレクションをイテレータに変換します。


* `iter` - これは、コレクションの各要素を各イテレーションで借用します。
  コレクション変更しないため、ループの後に再利用できます。

```rust, editable
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter() {
        match name {
            &"Ferris" => println!("There is a rustacean among us!"),  // 私達のようなRustaceanがいます
            _ => println!("Hello {}", name),
        }
    }
}
```

* `into_iter` - これはコレクションを消費し、各繰り返しで正確なデータを提供します。
  一度コレクションが消費されると、データを「move」し、もはや再利用できないようになります。

```rust, editable
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.into_iter() {
        match name {
            "Ferris" => println!("There is a rustacean among us!"),
            _ => println!("Hello {}", name),
        }
    }
}
```

* `iter_mut` - コレクションの各要素を可変的に借用し、コレクションをその場で変更できる
  ようにします。

```rust, editable
fn main() {
    let mut names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter_mut() {
        *name = match name {
            &mut "Ferris" => "There is a rustacean among us!",
            _ => "Hello",
        }
    }

    println!("names: {:?}", names);
}
```

上記の例では`match`分岐の型が、繰り返しのタイプの違いを示すキーになります。
タイプの違いによって、違った振る舞いができることが分かるでしょう。

### こちらも参照:

- [イテレータ][iter]

[iter]: ../trait/iter.md
