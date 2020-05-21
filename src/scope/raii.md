# RAII

Rustの変数はスタック上のデータを保持するだけでなく、リソースを*所有*します。
例えば、`Box<T>`はヒープ上のメモリを所有します。Rustは[RAII][raii](Resource
Acquisition Is Initialization、リソースの取得は初期化である)に従っているため、
オブジェクトがスコープを出ると、デストラクタが呼び出され、所有していたリソースが
開放されます。

この振る舞いによって、手動でメモリを開放しなくて良いため、*リソースリーク*が防げます。
もうメモリリークを気にする必要はないのです! 以下に例があります。

```rust,editable
// raii.rs
fn create_box() {
    // ヒープに整数を格納する
    let _box1 = Box::new(3i32);

    // `_box1`は破棄され、メモリが開放されます。
}

fn main() {
    // ヒープに整数を格納する
    let _box2 = Box::new(5i32);

    // ネストされたスコープ
    {
        // ヒープに整数を格納する
        let _box3 = Box::new(4i32);

        // `_box3`は破棄され、メモリが開放されます
    }

    // 遊びでたくさんメモリを確保してみる
    // 手動でメモリを開放する必要はありません!
    for _ in 0u32..1_000 {
        create_box();
    }

    // `_box2`が破棄され、メモリが開放されます
}
```

もちろん、[`valgrind`][valgrind]でメモリエラーを確認します。

```shell
$ rustc raii.rs && valgrind ./raii
==26873== Memcheck, a memory error detector
==26873== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==26873== Using Valgrind-3.9.0 and LibVEX; rerun with -h for copyright info
==26873== Command: ./raii
==26873==
==26873==
==26873== HEAP SUMMARY:
==26873==     in use at exit: 0 bytes in 0 blocks
==26873==   total heap usage: 1,013 allocs, 1,013 frees, 8,696 bytes allocated
==26873==
==26873== All heap blocks were freed -- no leaks are possible
==26873==
==26873== For counts of detected and suppressed errors, rerun with: -v
==26873== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 2 from 2)
```

ひとつもリークしていません!

## デストラクタ

Rustのデストラクタは[`Drop`]トレイトによって実装されています。デストラクタは
リソースがスコープを抜けた時に呼び出されます。このトレイトは、すべての型に実装
する必要はなく、デストラクタに独自のロジックを実装する必要がある場合のみ実装
します。

どのように[`Drop`]トレイトが動くかを見てみるため、下の例を実行してください。
`main`関数内の変数がスコープを抜けた時、独自のデストラクタが呼び出されます。

```rust,editable
struct ToDrop;

impl Drop for ToDrop {
    fn drop(&mut self) {
        println!("ToDrop is being dropped");
    }
}

fn main() {
    let x = ToDrop;
    println!("Made a ToDrop!");
}
```

### こちらも参照:

- [Box][box]

[raii]: https://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization
[box]: ../std/box.md
[valgrind]: http://valgrind.org/info/
[`Drop`]: https://doc.rust-lang.org/std/ops/trait.Drop.html
