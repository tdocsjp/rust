---
title: Lint グループ
---

# Lint グループ（要約）

lint グループとは、個々の lint を1つずつ有効・無効にする代わりに、関連する lint をまとめて1つの名前で切り替えられるようにしたものです。たとえば `rustc -D nonstandard-style` は、複数の命名規則に関する lint を一度に有効化します。

## Lint グループ一覧

| グループ | 説明 |
|-------|-------------|
| **warnings** | デフォルトで警告を発するよう設定されたすべての lint |
| **deprecated-safe** | 過去に誤って safe とマークされた関数 |
| **future-incompatible** | 将来の互換性に問題があるコード |
| **keyword-idents** | 将来のエディションでキーワードになる識別子 |
| **let-underscore** | 不正である可能性が高い、ワイルドカードの let 束縛 |
| **nonstandard-style** | 標準的な命名規則への違反 |
| **refining-impl-trait** | 実装による `impl Trait` の戻り値型の洗練 |
| **rust-2018-compatibility** | 2015 から 2018 エディションへの移行 lint |
| **rust-2018-idioms** | Rust 2018 の慣用的な機能へ促す |
| **rust-2021-compatibility** | 2018 から 2021 エディションへの移行 lint |
| **rust-2024-compatibility** | 2021 から 2024 エディションへの移行 lint |
| **unknown-or-malformed-diagnostic-attributes** | 未知または不正な形式の診断属性 |
| **unused** | 宣言されたが使われていないもの、または余分な構文 |

注意: `bad-style` は `nonstandard-style` の非推奨なエイリアスです。

---

本ページは [Lint Groups (stable)](https://doc.rust-lang.org/stable/rustc/lints/groups.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
