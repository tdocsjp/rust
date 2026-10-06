---
title: Result
---

# Enum Result

```rust
pub enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`Result` は、成功 (`Ok`) または失敗 (`Err`) のいずれかを表す型です。詳しくは[モジュールレベルのドキュメント](/stable/std/result/)を参照してください。

## バリアント

### Ok(T)

成功を表し、値を含む。

### Err(E)

エラーを表し、エラー値を含む。

---

メソッドの一覧（`is_ok`・`unwrap`・`map`・`and_then` など）とその使い分けは、[モジュールレベルのドキュメント](/stable/std/result/)の「メソッド概観」にまとめて翻訳しています。各メソッドの個別ページ（完全な説明文と例）は今後追加予定です。最新の個別ページは [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/result/enum.Result.html) を参照してください。

---

本ページは [`std::result::Result` (stable)](https://doc.rust-lang.org/stable/std/result/enum.Result.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
