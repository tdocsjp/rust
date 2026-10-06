---
title: Lints
---

# Lints（要約）

lint は、ソースコードの品質を向上させるためのツールです。Rust コンパイラはコンパイル中に lint を実行して潜在的な問題を検出し、設定に応じて警告・エラー・何も出さないのいずれかになります。たとえば `unused_variables` lint は、宣言されたが使われていない変数について警告します。

## 将来互換性 (future-incompatible) lint

コンパイラの変更が既存のコードを壊す場合、コンパイラは「future-incompatible」lint を発行します。最初は詳細な説明とトラッキングイシューへのリンクを伴う警告として現れ、将来のリリースでハードエラーになる前に、ユーザーがコードを修正する時間を与えます。

## この章の構成

- [レベル](levels.html) — lint の深刻度をどう設定するか
- [グループ](groups.html) — 関連する lint をまとめる
- [一覧](listing/index.html) — lint の網羅的なリファレンス

---

本ページは [Lints (stable)](https://doc.rust-lang.org/stable/rustc/lints/index.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
