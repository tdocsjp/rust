---
title: ExpandError
---

# Struct ExpandError

🔬 **nightly 限定の実験的 API** (`proc_macro_expand` [#90765](https://github.com/rust-lang/rust/issues/90765))

`TokenStream::expand_expr` から返されるエラーです。

## 定義

```rust
#[non_exhaustive]
pub struct ExpandError;
```

この構造体には `#[non_exhaustive]` が付いています。つまり、将来のバージョンでフィールドやバリアントが追加される可能性があります。

## トレイト実装

### 標準トレイト

- **Debug** - デバッグ整形のための `fmt` を実装
- **Display** - 表示整形のための `fmt` を実装
- **Error** - 次を含む、エラートレイトの完全な実装:
  - `source()` - 下位のエラー（存在する場合）を返す
  - `description()` - ⚠️ 1.42.0 から非推奨
  - `cause()` - ⚠️ 1.33.0 から非推奨
  - `provide()` - 🔬 エラーコンテキストへのアクセスのための nightly 限定の実験的 API

### スレッド安全性

- **!Send** - `Send` を実装していません
- **!Sync** - `Sync` を実装していません

### Auto Trait の実装

- **Freeze**
- **RefUnwindSafe**
- **Unpin**
- **UnsafeUnpin**
- **UnwindSafe**

## Blanket 実装

すべての型に利用可能な標準の blanket 実装:

- `Any` - `type_id()` による型の識別
- `Borrow<T>` - 不変の借用
- `BorrowMut<T>` - 可変の借用
- `From<T>` - `T` からの変換
- `Into<U>` - `U` への変換
- `ToString` - `String` への変換（`Display` トレイトが必要）
- `TryFrom<U>` - `U` からの失敗する可能性のある変換
- `TryInto<U>` - `U` への失敗する可能性のある変換

---

本ページは [`proc_macro::ExpandError` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.ExpandError.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
