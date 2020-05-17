# キャスト

Rustはプリミティブ型の暗黙的な変換(強制)を提供していません。しかし、
`as`キーワードで明示的な変換(キャスト)ができます。

整数型の変換に関する規則はCと基本的に同じですが、Cには未定義動作があり、
Rustはすべての型キャストがうまく定義されています。

```rust,editable,ignore,mdbook-runnable
// キャストでの桁溢れに関する警告をなくす。
#![allow(overflowing_literals)]

fn main() {
    let decimal = 65.4321_f32;

    // エラー! 暗黙的な変換はしません
    let integer: u8 = decimal;
    // FIXME ^ この行をコメントアウトしてください

    // 明示的な変換
    let integer = decimal as u8;
    let character = integer as char;

    // エラー! これは変換規則によって禁止されています。浮動小数点は文字に直接変換できません。
    let character = decimal as char;
    // FIXME ^ この行をコメントアウトしてください

    println!("Casting: {} -> {} -> {}", decimal, integer, character);

    // 値を符号なし整数型Tにキャストするときは、T::MAX + 1
    // を新しい型にフィットするように加減します。

    // 1000はu16にフィットしています。
    println!("1000 as a u16 is: {}", 1000 as u16);

    // 1000 - 256 - 256 - 256 = 232
    // 内部時には、最初の8最下位ビット(LSB)は保存され、
    // 残りの最上位ビットは切り捨てられます。
    println!("1000 as a u8 is : {}", 1000 as u8);
    // -1 + 256 = 255
    println!("  -1 as a u8 is : {}", (-1i8) as u8);

    // 正の数は、剰余と同じです。
    println!("1000 mod 256 is : {}", 1000 % 256);

    // 符号付き整数型に変換する時、(ビット処理)結果は対応する符号なし整数型への
    // キャストの結果と同じです。最上位ビットが1の時、その値は負です。

    // もちろん、これはすでにフィットしています。
    println!(" 128 as a i16 is: {}", 128 as i16);
    // 128 as u8 -> 128  これの8ビットでの2の補数は
    println!(" 128 as a i8 is : {}", 128 as i8);

    // 上の例を繰り返します。
    // 1000 as u8 -> 232
    println!("1000 as a u8 is : {}", 1000 as u8);
    // 232の2の補数は-24
    println!(" 232 as a i8 is : {}", 232 as i8);
}
```
