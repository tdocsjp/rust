---
title: 組み込みターゲット
---

# 組み込みターゲット（要約）

## 組み込みターゲットの一覧表示

`rustc --print target-list` を実行すると、`rustc` が自動的にコンパイルできる、利用可能なすべての組み込みターゲットが表示されます。

## ターゲットの使い方

次のように `--target` フラグで特定のターゲットへコンパイルします。

```bash
rustc --target <target-triple> <source-file>
```

## 概念

組み込みターゲットは、あらかじめサポートされているコンパイル対象で、通常は Rust チームが積極的にメンテナンスしているプラットフォームに対応します。ほとんどのターゲットには次が必要です。

- コンパイル済みの Rust 標準ライブラリ（クロスコンパイル用のビルド済みバージョンは `rustup` で入手できる）
- システムのリンカ
- 場合によってはその他のプラットフォーム固有のツール

個々のターゲットタプルをすべて覚える代わりに、`--print target-list` で利用可能な選択肢を調べ、自分のプラットフォームに合うものを選べます。

---

本ページは [Built-in Targets (stable)](https://doc.rust-lang.org/stable/rustc/targets/built-in.html) の要約の非公式日本語訳です。個々のターゲットの一覧は [doc.rust-lang.org](https://doc.rust-lang.org/stable/rustc/platform-support.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
