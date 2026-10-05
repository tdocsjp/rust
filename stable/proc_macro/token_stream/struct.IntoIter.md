---
title: IntoIter
---

# Struct IntoIter

```rust
pub struct IntoIter(/* private fields */);
```

**利用可能バージョン:** 1.29.0 から

`TokenStream` の `TokenTree` に対するイテレータです。このイテレーションは「浅い (shallow)」もので、たとえば区切られたグループの中には再帰せず、グループ全体を1つのトークンツリーとして返します。

## トレイト実装

### Clone
- `Clone` トレイトを実装
- 1.29.0 から利用可能

### Iterator
- 1.29.0 から利用可能
- **関連型:**
  - `Item = TokenTree`

**主なメソッド:**
- `fn next(&mut self) -> Option<TokenTree>` - イテレータを進め、次の値を返す
- `fn size_hint(&self) -> (usize, Option<usize>)` - 残りの長さの範囲を返す
- `fn count(self) -> usize` - イテレータを消費し、回数を数える

このイテレータは、`Iterator` トレイトから多数のメソッドも継承しています。たとえば:
- `map`、`filter`、`fold`、`collect`、`zip`、`chain`、`take`、`skip`
- `find`、`position`、`any`、`all`、`enumerate`、`peekable`
- その他多数のイテレータアダプタ・コンシューマ

## Auto Trait の実装

- **!Send** - スレッド間で送信できません
- **!Sync** - スレッド間で共有できません
- **Freeze** - Freeze 安全
- **RefUnwindSafe** - 巻き戻しに対して安全
- **Unpin** - 安全に move できます
- **UnsafeUnpin** - unsafe な unpin のサポート
- **UnwindSafe** - 巻き戻し中も安全

## Blanket 実装

すべての型に利用可能な標準トレイト実装:
- `Any`、`Borrow<T>`、`BorrowMut<T>`、`CloneToUninit`
- `From<T>`、`Into<U>`、`IntoIterator`、`ToOwned`
- `TryFrom<U>`、`TryInto<U>`

---

本ページは [`proc_macro::token_stream::IntoIter` (stable)](https://doc.rust-lang.org/stable/proc_macro/token_stream/struct.IntoIter.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
