---
title: LexError
---

# Struct LexError

```rust
pub struct LexError(/* private fields */);
```

**利用可能バージョン:** 1.15.0 から

`TokenStream::from_str` から返されるエラーです。含まれるエラーメッセージは、どのような意味においても安定していることは明示的に保証されておらず、Rust のバージョン間やコンパイルごとに変わる可能性があります。

## トレイト実装

### 基本トレイト

- **Debug** (1.15.0) - デバッグ整形のための `fmt()` メソッドを提供
- **Display** (1.44.0) - 表示整形のための `fmt()` メソッドを提供
- **Error** (1.44.0) - 標準のエラートレイトを実装し、次のメソッドを持つ:
  - `source()` - このエラーのより下位のソースを返す (1.30.0)
  - `description()` - 1.42.0 から非推奨
  - `cause()` - 1.33.0 から非推奨
  - `provide()` - エラーコンテキストへのアクセスのための nightly 限定の実験的 API

### マーカートレイト

- **!Send** (1.15.0) - `Send` を実装していません
- **!Sync** (1.15.0) - `Sync` を実装していません

## Auto Trait の実装

- **Freeze**
- **RefUnwindSafe**
- **Unpin**
- **UnsafeUnpin**
- **UnwindSafe**

## Blanket 実装

- `Any` - 型の識別
- `Borrow<T>` - 不変の借用
- `BorrowMut<T>` - 可変の借用
- `From<T>` - 失敗しない変換
- `Into<U>` - 他の型への失敗しない変換
- `ToString` - `Display` トレイトを通じた `String` への変換
- `TryFrom<U>` - `Infallible` エラー型を持つ失敗する可能性のある変換
- `TryInto<U>` - 他の型への失敗する可能性のある変換

---

本ページは [`proc_macro::LexError` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.LexError.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
