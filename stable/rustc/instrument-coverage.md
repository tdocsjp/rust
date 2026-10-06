---
title: Instrumentation ベースのコードカバレッジ
---

# Instrumentation ベースのコードカバレッジ（要約）

## 概要

Instrumentation ベースのコードカバレッジは、Rust のコードのどの部分が実行されたかを測定する、LLVM ベースのプロファイリング機構です。コンパイル済みコードにカウンタを自動的に注入し、カバレッジのマッピング情報をバイナリに埋め込むことで動作します。

## 仕組み

`-C instrument-coverage` が有効な場合、コンパイラは次を行います。

1. 関数や分岐に **LLVM intrinsic**（`llvm.instrprof.increment`）を注入し、実行時にカウンタを増やす
2. カウント対象のソースコード領域を定義する**カバレッジマップ**（LLVM Code Coverage Mapping Format v5 または v6）をバイナリに埋め込む

実行時にはカウンタの値が `.profraw` ファイルに書き出され、LLVM のツールがそれを人が読めるカバレッジレポートへ処理します。

## 主なワークフロー

### 1. カバレッジ付きでコンパイルする
```bash
RUSTFLAGS="-C instrument-coverage" cargo build
```

### 2. 計測済みバイナリを実行する
```bash
./target/debug/your-binary
# default_<signature>_<pid>.profraw が生成される
```

`LLVM_PROFILE_FILE` で出力ファイル名を制御できます。

```bash
LLVM_PROFILE_FILE="output.profraw" ./target/debug/your-binary
```

### 3. 生のプロファイルを索引化する
```bash
llvm-profdata merge -sparse output.profraw -o output.profdata
```

### 4. レポートを生成する
```bash
# 要約レポート
llvm-cov report --instr-profile=output.profdata --object ./binary

# ソース付きの詳細なカバレッジ
llvm-cov show --instr-profile=output.profdata --object ./binary \
  -Xdemangler=rustfilt --show-line-counts-or-regions
```

## 重要な要件・注意点

- **プロファイラランタイム**が必要。nightly の Rust にはデフォルトで含まれる。ソースからビルドする場合は `bootstrap.toml` で `profiler = true` を有効にする
- **LLVM との互換性**: LLVM 12 以降が必要。コンパイラとカバレッジツールのバージョンを揃えることが推奨される
- **シンボルのデマングル**: レポートで関数名を読みやすくするには `rustfilt` をインストールする
- **最適化との非互換性**: 一部のコンパイラオプションはカバレッジと組み合わせると非互換な LLVM IR を生成することがあるので、早めにテストする
- **シンボルマングリングの自動設定**: `-C instrument-coverage` は v0 シンボルマングリングを自動的に有効にする（レガシー形式より推奨される）

## カバレッジの指標

レポートは次の4つの統計を追跡します。

- **関数カバレッジ**: 実行された関数の割合
- **インスタンス化カバレッジ**: 実行された関数インスタンスの割合（ジェネリクス・マクロに関連）
- **行カバレッジ**: 実行された実行可能行の割合
- **領域カバレッジ**: 実行されたコード領域の割合（最も細かく、1行内の分岐も扱う）

## フラグのオプション

- `-C instrument-coverage` または `-C instrument-coverage=yes` — すべての関数を計測する（デフォルト）
- `-C instrument-coverage=no` — カバレッジ計測を無効にする
- `#[coverage(off)]` 属性で、関数ごとにカバレッジを無効化できる

---

本ページは [Instrumentation-based Code Coverage (stable)](https://doc.rust-lang.org/stable/rustc/instrument-coverage.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
