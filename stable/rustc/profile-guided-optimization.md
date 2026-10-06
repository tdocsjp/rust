---
title: プロファイルガイド最適化
---

# プロファイルガイド最適化（PGO）（要約）

## PGO とは

プロファイルガイド最適化 (Profile-Guided Optimization) は、プログラムの典型的な実行パターン（分岐の挙動など）に関するデータを収集し、その情報を使ってインライン化・コードレイアウト・レジスタ割り当てといったコンパイラの最適化を導きます。これにより、実際のワークロードに合わせた、より速いバイナリが生成されます。

## ワークフロー

PGO は4つの手順からなります。

1. **計測 (Instrument)**: `-Cprofile-generate=/path/to/data` でコンパイルする
2. **収集 (Collect)**: 計測済みのバイナリを実行し、`.profraw` ファイルを生成する
3. **統合 (Merge)**: `llvm-profdata merge` で `.profraw` を `.profdata` へ変換する
4. **最適化 (Optimize)**: `-Cprofile-use=/path/to/merged.profdata` で再コンパイルする

## 主な rustc フラグ

- `-Cprofile-generate=<path>` — 計測を有効にする
- `-Cprofile-use=<path>` — コンパイル時にプロファイリングデータを適用する

## 重要な注意点

- プロファイルデータの場所には**絶対パスを使う**こと。Cargo は様々なディレクトリから `rustc` を呼び出す
- 新しい PGO の実行前に**以前のデータを削除する**こと（古いプロファイリングデータを避けるため）
- PGO の効果を意味あるものにするため、**`-O`（リリースモード）でコンパイルする**
- `llvm-profdata` は `rustup component add llvm-tools-preview` でインストールする
- 過去のバグのため **Cargo 1.39 以降**が必要
- Cargo のワークフローでは、ビルドスクリプトを除外するために `RUSTFLAGS` 環境変数と `--target` フラグを使う

## 代替手段: cargo-pgo

より簡単なワークフローとして、コミュニティツールの `cargo-pgo` が `cargo pgo build` と `cargo pgo optimize` でこの一連の過程を自動化します。

---

本ページは [Profile-guided Optimization (stable)](https://doc.rust-lang.org/stable/rustc/profile-guided-optimization.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
