---
title: テスト
---

# テスト（要約）

## `--test` フラグ

`rustc` に `--test` を渡すと、クレートは次のような変更を加えてテストハーネスの実行可能ファイルとしてコンパイルされます。

1. **実行可能ファイルとしてビルド** — クレートは `bin` クレート種別になる
2. **libtest をリンク** — 標準ライブラリのテストハーネスに接続する
3. **main() を合成** — コマンドライン引数を処理してテストを実行する新しいエントリポイントを作成する（既存の `main` を置き換える）
4. **`test` cfg を有効化** — `#[cfg(test)]` による条件付きコンパイルを可能にする
5. **test/bench 関数を有効化** — `#[test]`・`#[bench]` 属性が付いた関数をコンパイルする

## 基本的な使い方

テストは `#[test]` 属性を付けたフリー関数として書きます。

```rust
#[test]
fn it_works() {
    assert_eq!(2 + 2, 4);
}
```

テストは、エラーなく戻れば**成功**、パニックするか非ゼロの `Result`/`Termination` 値を返せば**失敗**です。

## `#[test]` 属性との関係

`#[test]` 属性は、テストとして実行される関数をマークします。`--test` フラグなしでは、これらの関数は無視されます。`--test` ありでは、ハーネスによってコンパイル・実行されます。

## Cargo との統合

Cargo を使っているなら、[`cargo test`](/stable/cargo/commands/cargo-test.html) コマンドがこれをすべて自動で処理します。`rustc --test` を手動で呼ぶ必要はありません。Cargo はコンパイルと実行の過程を透過的に管理します。

---

本ページは [Tests (stable)](https://doc.rust-lang.org/stable/rustc/tests/index.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
