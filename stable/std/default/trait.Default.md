---
title: Default
---

# Trait Default（要約）

```rust
pub trait Default: Sized {
    fn default() -> Self;
}
```

`Default` トレイトは、妥当な既定値を持つ型のインスタンスを作成するための標準的な方法を提供します。設定オプションや、初期化が必要な状態を表す型に特に有用です。

## `#[derive(Default)]` の使用

構造体・enum に対する最も簡単な方法です。

```rust
#[derive(Default)]
struct SomeOptions {
    foo: i32,      // 既定値は 0
    bar: f32,      // 既定値は 0.0
}

let options: SomeOptions = Default::default();
```

**enum** では、`#[default]` でどの unit バリアントを既定にするか指定する必要があります。

```rust
#[derive(Default)]
enum Kind {
    #[default]
    A,
    B,
    C,
}
```

## よくある用途

**構造体更新記法** — 一部のフィールドだけ上書きし、残りは既定値のままにする:

```rust
let options = SomeOptions { foo: 42, ..Default::default() };
```

**既定値を持つオプショナルな値** — `unwrap_or_default()` のようなメソッドがこのトレイトを活用します。

```rust
let value: Option<String> = None;
let result = value.unwrap_or_default();  // 空の String を返す
```

**コレクションの初期化** — `Vec`、`HashMap`、`String` などのコレクションはすべて `Default` を実装しており、空のインスタンスを返します。

## 手動での実装

derive できないカスタム型には、手動で実装します。

```rust
impl Default for Kind {
    fn default() -> Self { Kind::A }
}
```

## 標準ライブラリでの実装状況

Rust は次の型に対して `Default` を実装しています。

- **プリミティブ**: 整数（既定値0）、浮動小数点数（0.0）、真偽値（false）、`char`（`'\0'`）
- **コレクション**: `Vec`、`HashMap`、`BTreeMap`、`String` など（すべて空）
- **スマートポインタ**: `Box`、`Arc`、`Rc`（`T` が `Default` を実装している場合）
- **同期プリミティブ**: `Mutex`、`RwLock`、`Condvar`
- **タプルと配列**: 32要素まで（内側の型が `Default` を実装している場合）

## Option との関係

```rust
impl<T> Default for Option<T> {
    fn default() -> Self { None }  // 常に None
}
```

これにより `Option::<String>::default()` が `None` になるパターンや、値が存在しないときに `unwrap_or_default()` で既定値を取り出すパターンが可能になります。

---

本ページは [`std::default::Default` (stable)](https://doc.rust-lang.org/stable/std/default/trait.Default.html) の要約の非公式日本語訳です。個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/default/trait.Default.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
