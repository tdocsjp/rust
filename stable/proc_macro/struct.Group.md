---
title: Group
---

# Struct Group

```rust
pub struct Group(/* private fields */);
```

区切られたトークンストリーム。区切り記号で囲まれた `TokenStream` を含みます。

## 関連関数

### `new`

```rust
pub fn new(delimiter: Delimiter, stream: TokenStream) -> Group
```

与えられた区切り記号とトークンストリームから新しい `Group` を作成します。スパンは初期状態では `Span::call_site()` に設定され、`set_span` で変更できます。

## メソッド

### `delimiter`

```rust
pub fn delimiter(&self) -> Delimiter
```

この `Group` の区切り記号を返します。

### `stream`

```rust
pub fn stream(&self) -> TokenStream
```

この `Group` の中で区切られているトークンの `TokenStream` を返します。返されるトークンストリームには区切り記号自体は含まれないことに注意してください。

### `span`

```rust
pub fn span(&self) -> Span
```

この `Group` 全体にわたる、区切り記号のスパンを返します。

### `span_open`

```rust
pub fn span_open(&self) -> Span
```

この group の開き区切り記号を指すスパンを返します。

### `span_close`

```rust
pub fn span_close(&self) -> Span
```

この group の閉じ区切り記号を指すスパンを返します。

### `set_span`

```rust
pub fn set_span(&mut self, span: Span)
```

この `Group` の区切り記号のスパンを設定しますが、内部のトークンのスパンは変更しません。このメソッドは `Group` レベルの区切りトークンのスパンにのみ影響し、内部のトークンには影響しません。

## トレイト実装

- **Clone**: クローンに対応
- **Debug**: デバッグ表示に対応
- **Display**: group を、損失なく変換できる文字列として表示します（`Delimiter::None` を持つ `TokenTree::Group` を除く）
- **From<Group>**: `Group` を `TokenTree` へ変換します
- **Extend<Group>**: `TokenStream` を `Group` の値で拡張できます
- **ToTokens**: トークンストリームへの変換
- **!Send と !Sync**: `Send` も `Sync` も実装していません

## Auto Trait の実装

- Freeze、RefUnwindSafe、Unpin、UnsafeUnpin、UnwindSafe

---

本ページは [`proc_macro::Group` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.Group.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
