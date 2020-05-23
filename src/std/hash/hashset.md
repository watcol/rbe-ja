# ハッシュセット

`HashSet`は、キーしか持たない`HashMap`で、`HashSet<T>`は、実際には
`HashMap<T, ()>`のラップです。

「何が特殊なの?」「キーを`Vec`に入れたら良いじゃないか。」と思うかもしれません。

`HashSet`特有の機能は、重複した要素を持たないことが保証されていることです。
これが他のコレクションとの大きな違いです。`HashSet`はその一つの実装にしか
過ぎません。(こちらも参照: [`BTreeSet`][treeset])

すでに存在する値を`HashSet`に挿入した場合はどうなるのでしょうか。(これは同じ
ハッシュを持つ新しい値を代入するのと同じなので)このとき、新しい値が古い値を
置き換えます。

これは2回以上同じことをしたくないときや、すでに知っているものをもう一度取得したくない
場合に役立ちます。

HashSetはさらに多くのことができます。

HashSetは以下の4つの操作を持っています(すべてイテレータを返します。)

* `union`: 2つの集合から重複しない要素を取り出す。

* `difference`: 最初の集合にあって、2つ目にない要素をすべて取り出す。

* `intersection`: 両方の集合にある要素を取り出す。

* `symmetric_difference`: 
2つの集合の内一つにはあるが、両方には無いものをすべて取り出す。

以下の例で全て試してみましょう:

```rust,editable,ignore,mdbook-runnable
use std::collections::HashSet;

fn main() {
    let mut a: HashSet<i32> = vec![1i32, 2, 3].into_iter().collect();
    let mut b: HashSet<i32> = vec![2i32, 3, 4].into_iter().collect();

    assert!(a.insert(4));
    assert!(a.contains(&4));

    // `HashSet::insert()`はすでに値が存在する時
    // falseを返す。
    assert!(b.insert(4), "Value 4 is already in set B!");
    // FIXME ^ この行をコメントアウトしてください。

    b.insert(5);

    // 要素の型が`Debug`を実装していれば、
    // そのコレクションも`Debug`を実装する。
    // 普通`[elem1, elem2, ...]`のように出力される。
    println!("A: {:?}", a);
    println!("B: {:?}", b);

    // [1, 2, 3, 4, 5]を順番に出力する。
    println!("Union: {:?}", a.union(&b).collect::<Vec<&i32>>());

    // これは[1]を出力する
    println!("Difference: {:?}", a.difference(&b).collect::<Vec<&i32>>());

    // これは[2, 3, 4]を順番に出力する。
    println!("Intersection: {:?}", a.intersection(&b).collect::<Vec<&i32>>());

    // [1, 5]を出力する。
    println!("Symmetric Difference: {:?}",
             a.symmetric_difference(&b).collect::<Vec<&i32>>());
}
```

(サンプルはHashSetの[ドキュメント][hash-set]から拝借しました。)

[treeset]: https://doc.rust-lang.org/std/collections/struct.BTreeSet.html
[hash-set]: https://doc.rust-lang.org/std/collections/struct.HashSet.html#method.difference
