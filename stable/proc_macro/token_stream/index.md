---
title: token_stream
---

# Module token_stream

**利用可能バージョン:** 1.29.0 から

`TokenStream` 型のための、イテレータなどの公開実装詳細です。

## 構造体

### [IntoIter](./struct.IntoIter.md)

`TokenStream` の `TokenTree` に対するイテレータです。このイテレーションは「浅い (shallow)」もので、区切られたグループの中には再帰しません。グループ全体を1つのトークンツリーとして返します。

---

本ページは [`proc_macro::token_stream` (stable)](https://doc.rust-lang.org/stable/proc_macro/token_stream/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
