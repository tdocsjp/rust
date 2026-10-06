---
title: Option
---

# Enum Option

```rust
pub enum Option<T> {
    None,
    Some(T),
}
```

`Option` 型。詳しくは[モジュールレベルのドキュメント](/stable/std/option/)を参照してください。

## バリアント

### None

値なし。

### Some(T)

型 `T` の何らかの値。

---

メソッドの一覧（`is_some`・`unwrap`・`map`・`and_then`・`take` など100以上のメソッド）とその使い分けは、[モジュールレベルのドキュメント](/stable/std/option/)の「メソッド概観」にまとめて翻訳しています。各メソッドの個別ページ（完全な説明文と例）は今後追加予定です。最新の個別ページは [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/option/enum.Option.html) を参照してください。

---

本ページは [`std::option::Option` (stable)](https://doc.rust-lang.org/stable/std/option/enum.Option.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
