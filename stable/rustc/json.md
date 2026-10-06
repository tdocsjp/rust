---
title: JSON 出力
---

# JSON 出力（要約）

## 目的

rustc の JSON 出力形式は、機械が読める診断情報・コンパイル情報を提供し、ツールやビルドシステムがテキスト解析に頼らずプログラム的にコンパイラのメッセージを扱えるようにします。

## 有効化の方法

- **基本**: `--error-format=json` フラグを使う
- **高度な設定**: `--json` フラグで、どの種類のメッセージを生成するか、その形式を制御する
- 出力は標準エラー出力へ1行ずつ出力される

## 主なメッセージ種別

### 1. 診断 (Diagnostics)

コンパイルのエラー・警告・ノートを含む、主要なメッセージ種別です。各診断には以下が含まれます。

- **message**: 診断の本文
- **level**: "error"、"warning"、"note"、"help"、"failure-note"、"error: internal compiler error" のいずれか
- **code**: 一意の識別子と任意の説明
- **spans**: 問題の発生箇所を示す、バイト/行/列オフセットを伴うソースコードの位置
- **children**: 文脈・ヒント・提案を提供する関連する診断メッセージ
- **suggested_replacement**: 適用可能性のレベル（MachineApplicable、MaybeIncorrect、HasPlaceholders、Unspecified）を伴う、任意の修正案

### 2. アーティファクト (`--json=artifacts`)

生成されたファイルの通知。`artifact`（生成されたファイル名）と `emit`（アーティファクトの種類: link、dep-info、metadata、asm、llvm-ir、llvm-bc、mir、obj）を含みます。

### 3. 将来互換性レポート (`--json=future-incompat`)

`#[allow]` や `--cap-lints` で抑制されていても、将来の Rust バージョンでハードエラーになる可能性がある警告。

### 4. 未使用の依存の通知 (`--json=unused-externs` / `unused-externs-silent`)

`--extern` で指定された、使われていないクレート依存を報告します。lint レベルを設定可能です。

### 5. タイミング (`--timings`、`-Zunstable-options` とともに)

コンパイルの各段階（開始/終了）をマイクロ秒単位のタイムスタンプで示すイベント。

## 解析時の注意

- すべてのメッセージには、形式を区別するための `$message_type` フィールドが含まれる
- 前方互換性を保つこと: 任意の値は `null` になることがあり、新しいフィールドが追加されることもあり、列挙された値が増えることもある
- Rust の開発者は解析のサポートとして [`cargo_metadata`](https://crates.io/crates/cargo_metadata) クレートを使える

---

本ページは [JSON Output (stable)](https://doc.rust-lang.org/stable/rustc/json.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
