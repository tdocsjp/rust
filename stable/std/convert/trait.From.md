---
title: From / Into
---

# Trait From・Into（要約）

```rust
pub trait From<T>: Sized {
    fn from(value: T) -> Self;
}
```

`From<T>` トレイトは、入力を消費する、失敗しない・情報を失わない値から値への変換を可能にします。これは `Into<U>` の逆であり、Rust で型の間を変換する標準的な方法です。

## 設計上の要点: From を実装すれば Into も手に入る

```rust
// From を実装すると、blanket 実装のおかげで Into も自動的に手に入る
impl From<&str> for String {
    fn from(s: &str) -> Self { /* ... */ }
}

// これで Into も自動的に使える:
let s: String = "hello".into();  // 自動的に動く
```

クレートをまたぐ変換で Rust 1.41 より前をターゲットにする場合を除き、**`Into` より常に `From` を実装するようにしてください**。

## `?` 演算子によるエラーハンドリング

`?` 演算子は `From::from` を使って、エラー型を自動的に変換します。

```rust
impl From<io::Error> for MyError {
    fn from(error: io::Error) -> Self {
        MyError::Io(error)
    }
}

impl From<num::ParseIntError> for MyError {
    fn from(error: num::ParseIntError) -> Self {
        MyError::Parse(error)
    }
}

fn process() -> Result<i32, MyError> {
    let data = fs::read_to_string("file")?;  // io::Error → MyError
    let num: i32 = data.trim().parse()?;     // ParseIntError → MyError
    Ok(num)
}
```

## 典型的な使用パターン

```rust
// 文字列の変換
let s: String = String::from("hello");
let s: String = "hello".into();

// ネットワーク型
let addr: IpAddr = Ipv4Addr::new(127, 0, 0, 1).into();

// Box への変換
let boxed: Box<str> = String::from("hello").into();
```

## From を実装すべき場面

変換は次のようであるべきです。

- **失敗しない**（失敗する可能性があるなら `TryFrom` を使う）
- **情報を失わない**（情報の損失がない。`i32::from(u16)` は ✓ だが `u16::from(u32)` は ✗）
- **値を保つ**（概念的に同じものを表す。たとえば `1_i16` と `1.0_f32` はどちらも「1」を意味する）
- **自明**（それが唯一の合理的な変換である。そうでなければ名前付きのメソッドを使う）

**注意:** `From` はパニックしてはいけません。完全な変換のためだけに使ってください。

---

本ページは [`std::convert::From` (stable)](https://doc.rust-lang.org/stable/std/convert/trait.From.html) の要約の非公式日本語訳です。個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/convert/trait.From.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
