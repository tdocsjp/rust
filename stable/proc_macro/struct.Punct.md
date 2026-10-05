---
title: Punct
---

# Struct Punct

```rust
pub struct Punct(/* private fields */);
```

`Punct` は `+`、`-`、`#` のような単一の句読点文字です。

`+=` のような複数文字の演算子は、異なる `Spacing` を持つ2つの `Punct` インスタンスとして表現されます。

## 関連関数

### `new`

```rust
pub fn new(ch: char, spacing: Spacing) -> Punct
```

与えられた文字と spacing から新しい `Punct` を作成します。`ch` 引数は、言語が許容する正当な句読点文字でなければなりません。そうでない場合、この関数はパニックします。

返される `Punct` はデフォルトで `Span::call_site()` のスパンを持ちますが、下記の `set_span` メソッドでさらに設定できます。

## メソッド

### `as_char`

```rust
pub fn as_char(&self) -> char
```

この句読点文字の値を `char` として返します。

### `spacing`

```rust
pub fn spacing(&self) -> Spacing
```

この句読点文字の spacing を返します。これは、続くトークンと結合して複数文字の演算子を形成しうるか (`Joint`)、あるいはその演算子がそこで確実に終わっているか (`Alone`) を示します。

### `span`

```rust
pub fn span(&self) -> Span
```

この句読点文字のスパンを返します。

### `set_span`

```rust
pub fn set_span(&mut self, span: Span)
```

この句読点文字のスパンを設定します。

## トレイト実装

### Clone

```rust
fn clone(&self) -> Punct
```

値の複製を返します。

### Debug

```rust
fn fmt(&self, f: &mut Formatter<'_>) -> Result
```

与えられたフォーマッタを使って値を整形します。

### Display

句読点文字を、損失なく同じ文字へ戻せるはずの文字列として表示します。

```rust
fn fmt(&self, f: &mut Formatter<'_>) -> Result
```

### Extend<Punct> for TokenStream

```rust
fn extend<T: IntoIterator<Item = Punct>>(&mut self, iter: T)
```

イテレータの内容でコレクションを拡張します。

### From<Punct> for TokenTree

```rust
fn from(g: Punct) -> TokenTree
```

入力された型からこの型へ変換します。

### PartialEq<char> for Punct

```rust
fn eq(&self, rhs: &char) -> bool
```

等価演算子 `==`。

### PartialEq<Punct> for char

```rust
fn eq(&self, rhs: &Punct) -> bool
```

等価演算子 `==`。

### ToTokens

```rust
fn to_tokens(&self, tokens: &mut TokenStream)
fn to_token_stream(&self) -> TokenStream
fn into_token_stream(self) -> TokenStream
```

`self` を直接 `TokenStream` オブジェクトへ変換します。

## Auto Trait の実装

- **!Send** - `Punct` は `Send` を実装していません
- **!Sync** - `Punct` は `Sync` を実装していません
- **Freeze**
- **RefUnwindSafe**
- **Unpin**
- **UnsafeUnpin**
- **UnwindSafe**

---

本ページは [`proc_macro::Punct` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.Punct.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
