# use

`use`宣言を使うと、変数のスコープを絶対名で指定する必要がなくなります。

```rust,editable
// 使われていないコードの警告をなくす属性
#![allow(dead_code)]

enum Status {
    Rich,
    Poor,
}

enum Work {
    Civilian,
    Soldier,
}

fn main() {
    // 明示的に名前を指定して`use`して、
    // 絶対名で指定しなくても使用できるようになる。
    use crate::Status::{Poor, Rich};
    // `Work`の中の名前をすべて自動的に`use`する。
    use crate::Work::*;

    // `Status::Poor`と等しい。
    let status = Poor;
    // `Work::Civilian`と等しい。
    let work = Civilian;

    match status {
        // 上で`use`しているので、スコープは不要です。
        Rich => println!("The rich have lots of money!"),  // 富豪はたくさんの金を持っています!
        Poor => println!("The poor have no money..."),  // 貧民は金を持っていません...
    }

    match work {
        // これも同じです。
        Civilian => println!("Civilians work!"),  // 民間人は働きます!
        Soldier  => println!("Soldiers fight!"),  // 兵士は戦います!
    }
}
```

### こちらも参照:

[`match`][match]と[`use`][use] 

[use]: ../../mod/use.md
[match]: ../../flow_control/match.md
