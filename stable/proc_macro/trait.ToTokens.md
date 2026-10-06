---
title: ToTokens
---

# Trait ToTokens

🔬 **nightly 限定の実験的 API** (`proc_macro_totokens` [#130977](https://github.com/rust-lang/rust/issues/130977))

[`quote!`](./macro.quote.md) の呼び出しの中に埋め込める型です。

## トレイト定義

```rust
pub trait ToTokens {
    // 必須メソッド
    fn to_tokens(&self, tokens: &mut TokenStream);

    // 提供されるメソッド
    fn to_token_stream(&self) -> TokenStream { ... }
    fn into_token_stream(self) -> TokenStream
       where Self: Sized { ... }
}
```

## 必須メソッド

### `to_tokens(&self, tokens: &mut TokenStream)`

与えられた `TokenStream` へ `self` を書き込みます。これが実装しなければならないコアのメソッドです。

`std::cmp::PartialEq` のような Rust のパスを表す構造体に対する**実装例**:

```rust
#![feature(proc_macro_totokens)]

use std::iter;
use proc_macro::{Spacing, Punct, TokenStream, TokenTree, ToTokens};

pub struct Path {
    pub global: bool,
    pub segments: Vec<PathSegment>,
}

impl ToTokens for Path {
    fn to_tokens(&self, tokens: &mut TokenStream) {
        for (i, segment) in self.segments.iter().enumerate() {
            if i > 0 || self.global {
                // ダブルコロン `::`
                tokens.extend(iter::once(TokenTree::from(Punct::new(':', Spacing::Joint))));
                tokens.extend(iter::once(TokenTree::from(Punct::new(':', Spacing::Alone))));
            }
            segment.to_tokens(tokens);
        }
    }
}
```

## 提供されるメソッド

### `to_token_stream(&self) -> TokenStream`

`self` を直接 `TokenStream` オブジェクトへ変換します。これは `to_tokens` を使って暗黙に実装され、便利メソッドとして機能します。

### `into_token_stream(self) -> TokenStream`

`self` を（所有権を取って）直接 `TokenStream` オブジェクトへ変換します。これは `to_tokens` を使って暗黙に実装され、便利メソッドとして機能します。

## 外部の型への実装

このトレイトは次に対して実装されています。

**プリミティブ型:**
- 整数型: `u8`、`u16`、`u32`、`u64`、`u128`、`usize`、`i8`、`i16`、`i32`、`i64`、`i128`、`isize`
- 浮動小数点型: `f32`、`f64`
- `bool`、`char`、`str`

**文字列型:**
- `String`
- `CStr`、`CString`

**コンテナ型:**
- `&T`（参照）
- `&mut T`（可変参照）
- `Box<T>`
- `Rc<T>`
- `Cow<'_, T>`
- `Option<T>`

## 実装先

proc_macro の次の型に対して実装されています。
- `TokenStream`
- `TokenTree`
- `Group`
- `Ident`
- `Literal`
- `Punct`

## dyn 互換性

このトレイトは dyn 互換です。つまりトレイトオブジェクトとして使用できます。

---

本ページは [`proc_macro::ToTokens` (stable)](https://doc.rust-lang.org/stable/proc_macro/trait.ToTokens.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
