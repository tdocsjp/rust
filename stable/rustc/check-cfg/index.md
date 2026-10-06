---
title: 条件付き設定のチェック
---

# 条件付き設定のチェック（要約）

## 目的

`--check-cfg` フラグは、コード中のすべての `#[cfg]` 属性が、あらかじめ定義された期待される設定名・値の一覧に一致することを rustc に検証させます。これは、異なるターゲットプラットフォームやフィーチャーをまたいだ条件付きコンパイルにおける、タイプミスや不整合を見つけるのに役立ちます。

## 既知の名前 とカスタムの名前

**既知の名前**（`--check-cfg` を使うと暗黙に利用可能）:
- ターゲット関連: `target_os`、`target_arch`、`target_env`、`target_pointer_width` など
- コンパイラフラグ: `debug_assertions`、`overflow_checks`、`panic`
- ツール: `clippy`、`rustfmt`、`miri`、`doc`、`doctest`
- フィーチャーゲート: `sanitize`、`relocation_model`

**カスタムの名前**は `--check-cfg` で明示的に宣言する必要があります。

## 基本構文

```bash
rustc --check-cfg 'cfg(name, values("value1", "value2"))'
```

主なバリエーション:
- `cfg(name)` または `cfg(name, values(none()))` — 値を期待しない名前
- `cfg(name, values())` — 名前は認識するが、どの値でも警告する
- `cfg(name, values(any()))` — 名前を認識し、どの値も許可する
- `cfg()` — 特定の期待なしでチェックを有効にする
- 複数の名前: `cfg(name1, name2, values("val1", "val2"))`

## `unexpected_cfgs` lint

rustc が、チェック対象の設定に一致しない `#[cfg(...)]` に遭遇すると:
- `unexpected_cfgs` lint が発行される（デフォルトは警告レベル）
- `#[cfg]`、`#[cfg_attr]`、`#[link(cfg(...))]`、`cfg!(...)` マクロに影響する
- `--cfg` コマンドラインフラグ自体は、現時点ではチェックされ**ない**

## 例

```bash
rustc --check-cfg 'cfg(feature, values("lion", "zebra"))' \
      --cfg 'feature="lion"' example.rs
```

- ✓ `#[cfg(feature = "lion")]` — 期待される値
- ✓ `#[cfg(feature = "zebra")]` — 期待される値（実行時には偽だが）
- ✗ `#[cfg(feature = "platypus")]` — **警告**: 予期しない値
- ✗ `#[cfg(feechure = "lion")]` — **警告**: 予期しない名前

---

本ページは [Checking Conditional Configurations (stable)](https://doc.rust-lang.org/stable/rustc/check-cfg.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
