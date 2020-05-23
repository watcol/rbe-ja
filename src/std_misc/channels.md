# チャンネル

Rustはスレッド間で同期するためのチャンネルを提供しています。チャンネルを
使えば、情報を2つのスレッド(`Sender`と`Receiver`)間で一方的に送信できます。

```rust,editable
use std::sync::mpsc::{Sender, Receiver};
use std::sync::mpsc;
use std::thread;

static NTHREADS: i32 = 3;

fn main() {
    // チャンネルは`Sender<T>`と`Receiver<T>`という2つの地点を持っていて、
    // `T`は送信する情報の型です。
    // (型注釈は必要ないものです)
    let (tx, rx): (Sender<i32>, Receiver<i32>) = mpsc::channel();
    let mut children = Vec::new();

    for id in 0..NTHREADS {
        // 送り手(Sender)はコピーできます。
        let thread_tx = tx.clone();

        // それぞれのスレッドがチャンネルを使ってデータを送信します。
        let child = thread::spawn(move || {
            // このスレッドは`thread_tx`の所有権を必要とします。
            // それぞれのスレッドがキューにデータを送ります。
            thread_tx.send(id).unwrap();

            // 送ることは制限された操作ではないので、スレッドは
            // 即座に次の操作ができます。
            println!("thread {} finished", id);
        });

        children.push(child);
    }

    // ここにすべてのメッセージが溜まっています
    let mut ids = Vec::with_capacity(NTHREADS as usize);
    for _ in 0..NTHREADS {
        // `recv`メソッドはチャンネルからのメッセージを一つとります。
        // `recv`はメッセージが届いていないときに操作がブロックされます。
        ids.push(rx.recv());
    }
    
    // 残りの作業を終えるため、スレッドの終了を待つ。
    for child in children {
        child.join().expect("oops! the child thread panicked");
    }

    // メッセージを贈られた順に並べる。
    println!("{:?}", ids);
}
```
