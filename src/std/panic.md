# `panic!`

`panic!`マクロでパニックを生成し、スタックを巻き戻すことができます。
スタックを巻き戻すときは、スレッド内の全てのリソースのデストラクタを
呼び出してメモリを開放します。

プログラムを1スレッドで管理しているので、`panic!`はパニックメッセージ
を表示して終了します。

```rust,editable,ignore,mdbook-runnable
// 整数除算(/)の再実装
fn division(dividend: i32, divisor: i32) -> i32 {
    if divisor == 0 {
        // 0で割るとパニックする
        panic!("division by zero");
    } else {
        dividend / divisor
    }
}

// `main`タスク
fn main() {
    // ヒープに整数を確保する
    let _x = Box::new(0i32);

    // この操作は失敗する。
    division(3, 0);

    println!("This point won't be reached!");  // ここにはたどり着きません!

    // `_x`はここで開放されるはずです。
}
```

`panic!`がメモリリークを起こしていないことを確認しましょう。

```shell
$ rustc panic.rs && valgrind ./panic
==4401== Memcheck, a memory error detector
==4401== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==4401== Using Valgrind-3.10.0.SVN and LibVEX; rerun with -h for copyright info
==4401== Command: ./panic
==4401== 
thread '<main>' panicked at 'division by zero', panic.rs:5
==4401== 
==4401== HEAP SUMMARY:
==4401==     in use at exit: 0 bytes in 0 blocks
==4401==   total heap usage: 18 allocs, 18 frees, 1,648 bytes allocated
==4401== 
==4401== All heap blocks were freed -- no leaks are possible
==4401== 
==4401== For counts of detected and suppressed errors, rerun with: -v
==4401== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```
