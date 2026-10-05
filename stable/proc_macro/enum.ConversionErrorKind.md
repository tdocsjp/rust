---
title: ConversionErrorKind
---

# Enum ConversionErrorKind

```rust
#[non_exhaustive]
pub enum ConversionErrorKind {
    FailedToUnescape(EscapeError),
    InvalidLiteralKind,
}
```

🔬 **nightly 限定の実験的 API** (`proc_macro_value` [#136652](https://github.com/rust-lang/rust/issues/136652))

リテラルのエスケープ解除後の値を取得しようとしたときに返されるエラーです。

## バリアント（非網羅的）

この enum は非網羅的 (`#[non_exhaustive]`) です。非網羅的な enum には将来バリアントが追加される可能性があります。そのため、非網羅的な enum のバリアントに対してマッチさせる際は、将来のバリアントに対応するためのワイルドカードアームを追加しなければなりません。

### FailedToUnescape([EscapeError](./enum.EscapeError.md))

🔬 **nightly 限定の実験的 API** (`proc_macro_value` [#136652](https://github.com/rust-lang/rust/issues/136652))

リテラルのエスケープ解除に失敗しました。詳しくは [`EscapeError`](./enum.EscapeError.md) を参照してください。

### InvalidLiteralKind

🔬 **nightly 限定の実験的 API** (`proc_macro_value` [#136652](https://github.com/rust-lang/rust/issues/136652))

誤った種類のリテラルを変換しようとしました。

## トレイト実装

### Debug

```rust
impl Debug for ConversionErrorKind
```

#### fn fmt(&self, f: &mut Formatter<'_>) -> Result

与えられたフォーマッタを使って値を整形します。

### Eq

```rust
impl Eq for ConversionErrorKind
```

### PartialEq

```rust
impl PartialEq for ConversionErrorKind
```

#### fn eq(&self, other: &ConversionErrorKind) -> bool

等価演算子 `==`。

#### fn ne(&self, other: &Rhs) -> bool

不等価演算子 `!=`。

### StructuralPartialEq

```rust
impl StructuralPartialEq for ConversionErrorKind
```

## Auto Trait の実装

- **Freeze**
- **RefUnwindSafe**
- **Send**
- **Sync**
- **Unpin**
- **UnsafeUnpin**
- **UnwindSafe**

## Blanket 実装

`Any`、`Borrow<T>`、`BorrowMut<T>`、`From<T>`、`Into<U>`、`TryFrom<U>`、`TryInto<U>` に対する標準トレイト実装。

---

本ページは [`proc_macro::ConversionErrorKind` (stable)](https://doc.rust-lang.org/stable/proc_macro/enum.ConversionErrorKind.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
