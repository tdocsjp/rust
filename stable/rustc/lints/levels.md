---
title: Lint レベル
---

# Lint レベル（要約）

## 6つの lint レベル

rustc は lint を6段階の設定可能なレベルに分類します。

1. **allow** — lint を抑制する（多くの lint のデフォルト）
2. **warn** — 警告を出す（一部の lint のデフォルト）
3. **expect** — lint を抑制しつつ、実際に発生することを確認する。発生しなければ `unfulfilled_lint_expectations` が発生する
4. **deny** — エラーにする（デフォルトの挙動とは異なり設定可能）
5. **force-warn** — 上書きできない強制的な警告にする
6. **forbid** — （`--cap-lints` を除き）下げられない強制的なエラーにする

## lint レベルの設定方法

### コンパイラフラグによる設定

`-A`、`-W`、`-D`、`-F`、`--force-warn` フラグを使います。

```bash
rustc lib.rs -W missing-docs      # warn
rustc lib.rs -D missing-docs      # deny（エラー）
rustc lib.rs -F missing-docs      # forbid
rustc lib.rs -A unused-variables  # allow
```

複数の lint・フラグを組み合わせられ、**後のフラグが前のフラグを上書きします**。

```bash
rustc lib.rs -D unused-variables -A unused-variables  # allow が勝つ
```

### 属性による設定

クレートレベル・項目レベルの属性を使います。

```rust
#![warn(missing_docs)]
#![deny(unused_variables)]

#[allow(unused_variables)]
fn foo() {}
```

複数の lint の指定や `reason` パラメータもサポートします。

```rust
#[allow(unused_mut, reason = "modified on some platforms")]
let mut x = 5;
```

## Lint のキャッピング

`--cap-lints LEVEL` は lint の上限レベルを設定し、それ以上への引き上げを防ぎます。

```bash
rustc lib.rs --cap-lints warn  # deny であっても warn になる
rustc lib.rs --cap-lints allow # force-warn を除くすべての lint を抑制する
```

Cargo は依存先の警告を抑制するためにこれを使っています。

## 優先順位（高い順）

1. `--force-warn`（forbid の文脈を除き、すべてを上書きする）
2. `--cap-lints`（属性や、ほとんどの CLI フラグを上書きする）
3. CLI フラグ（`-D`、`-W`、`-F`、`-A`） — 一番右のフラグが勝つ
4. ソース中の属性 — 内側・後の属性が外側・前の属性を上書きする
5. デフォルトの lint レベル

**例外**: 一度 `forbid` になった lint は下げられません（`deny` は forbid の文脈内でも許可されますが無視されます）。

---

本ページは [Lint Levels (stable)](https://doc.rust-lang.org/stable/rustc/lints/levels.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
