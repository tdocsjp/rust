---
title: is_available
---

# Function is_available

```rust
pub fn is_available() -> bool
```

**利用可能バージョン:** 1.57.0 から

## 説明

proc_macro が現在実行中のプログラムからアクセス可能になっているかどうかを判定します。

proc_macro クレートは、手続き的マクロの実装の内部でのみ使用されることを意図しています。このクレート内のすべての関数は、ビルドスクリプトや単体テスト、通常の Rust バイナリなど、手続き的マクロの外から呼び出された場合にパニックします。

マクロとしての用途とマクロ以外の用途の両方をサポートするように設計された Rust ライブラリを考慮して、`proc_macro::is_available()` は、proc_macro の API を使うために必要な基盤が現在利用可能かどうかを、パニックせずに検出する方法を提供します。手続き的マクロの内部から呼び出された場合は `true` を、それ以外のバイナリから呼び出された場合は `false` を返します。

---

本ページは [`proc_macro::is_available` (stable)](https://doc.rust-lang.org/stable/proc_macro/fn.is_available.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
