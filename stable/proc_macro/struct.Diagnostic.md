---
title: Diagnostic
---

# Struct Diagnostic

```rust
pub struct Diagnostic { /* private fields */ }
```

🔬 **nightly 限定の実験的 API** (トラッキングイシュー [#54140](https://github.com/rust-lang/rust/issues/54140))

診断メッセージと、それに関連する子メッセージを表す構造体です。

## 関連関数

### `new`

```rust
pub fn new<T: Into<String>>(level: Level, message: T) -> Diagnostic
```

与えられた `level` と `message` を持つ、新しい diagnostic を作成します。

### `spanned`

```rust
pub fn spanned<S, T>(spans: S, level: Level, message: T) -> Diagnostic
where
    S: MultiSpan,
    T: Into<String>
```

与えられた `level` と `message` を持ち、指定した `spans` を指す、新しい diagnostic を作成します。

## メソッド

### メッセージとレベルの管理
- **`level(&self) -> Level`** - diagnostic のレベルを返す
- **`set_level(&mut self, level: Level)`** - レベルを設定する
- **`message(&self) -> &str`** - メッセージを返す
- **`set_message<T: Into<String>>(&mut self, message: T)`** - メッセージを設定する

### スパンの管理
- **`spans(&self) -> &[Span]`** - スパンを返す
- **`set_spans<S: MultiSpan>(&mut self, spans: S)`** - スパンを設定する

### 子 diagnostic
- **`children(&self) -> Children<'_>`** - 子 diagnostic に対するイテレータを返す

### 子 diagnostic の追加

**error レベル:**
- `span_error<S, T>(self, spans: S, message: T) -> Diagnostic` - スパンあり
- `error<T: Into<String>>(self, message: T) -> Diagnostic` - スパンなし

**warning レベル:**
- `span_warning<S, T>(self, spans: S, message: T) -> Diagnostic` - スパンあり
- `warning<T: Into<String>>(self, message: T) -> Diagnostic` - スパンなし

**note レベル:**
- `span_note<S, T>(self, spans: S, message: T) -> Diagnostic` - スパンあり
- `note<T: Into<String>>(self, message: T) -> Diagnostic` - スパンなし

**help レベル:**
- `span_help<S, T>(self, spans: S, message: T) -> Diagnostic` - スパンあり
- `help<T: Into<String>>(self, message: T) -> Diagnostic` - スパンなし

### 発行
- **`emit(self)`** - diagnostic を発行する

## トレイト実装

- **Clone** - diagnostic のクローンに対応
- **Debug** - デバッグ整形に対応

## Auto Trait の実装

- **!Send** - `Send` ではない
- **!Sync** - `Sync` ではない
- **Freeze、RefUnwindSafe、Unpin、UnsafeUnpin、UnwindSafe** - いずれも実装される

## Blanket 実装

`Any`、`Borrow<T>`、`BorrowMut<T>`、`CloneToUninit`、`From<T>`、`Into<U>`、`ToOwned`、`TryFrom<U>`、`TryInto<U>` に対する標準トレイト実装。

---

本ページは [`proc_macro::Diagnostic` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.Diagnostic.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
