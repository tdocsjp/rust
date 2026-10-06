---
title: MultiSpan
---

# Trait MultiSpan

```rust
pub trait MultiSpan {
    // 必須メソッド
    fn into_spans(self) -> Vec<Span>;
}
```

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

`Span` の集合に変換できる型によって実装されるトレイトです。

## 必須メソッド

### fn into_spans(self) -> Vec<Span>

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

`self` を `Vec<Span>` へ変換します。

## dyn 互換性

このトレイトは dyn 互換です。

_以前の Rust のバージョンでは、dyn 互換性は「object safety（オブジェクト安全性）」と呼ばれていました。_

## 外部の型への実装

### impl MultiSpan for Vec<Span>

```rust
fn into_spans(self) -> Vec<Span>
```

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

### impl<'a> MultiSpan for &'a [Span]

```rust
fn into_spans(self) -> Vec<Span>
```

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

## 実装先

### impl MultiSpan for Span

---

本ページは [`proc_macro::MultiSpan` (stable)](https://doc.rust-lang.org/stable/proc_macro/trait.MultiSpan.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
