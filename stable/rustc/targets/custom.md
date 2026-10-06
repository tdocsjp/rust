---
title: カスタムターゲット
---

# カスタムターゲット（要約）

## カスタムターゲット仕様とは

カスタムターゲット仕様は、`rustc` がまだサポートしていないターゲットを定義・ビルドできるようにする **JSON ファイル**です。プラットフォーム固有のコンパイル設定を定義します。

## ターゲット仕様の確認

```bash
# ホストターゲットの場合
rustc +nightly -Z unstable-options --print target-spec-json

# 特定のターゲットの場合
rustc +nightly -Z unstable-options --target=wasm32-unknown-unknown --print target-spec-json
```

## 使い方

`--target` オプションは次の順序で検索します。

1. **組み込みターゲット** — TARGET が標準の Rust ターゲットに一致する場合
2. **ファイルパス** — TARGET が JSON ファイルへのパスである場合
3. **RUST_TARGET_PATH** — コロン区切りのディレクトリから `TARGET.json` を探す

## 要件

- Cargo の **`build-std` フィーチャー**（unstable）が必要
- JSON スキーマは sysroot の `etc/target-spec-json-schema.json` にあるか、次で確認できる:
```bash
rustc +nightly -Zunstable-options --print target-spec-json-schema
```

## 重要な制約

⚠️ **unstable かつサポート外:**

- ターゲット JSON のプロパティは**安定していません**。変更される可能性があります
- スキーマの存在や命名は保証されません
- カスタムターゲットを使う際は**必ずコンパイラのバージョンを固定してください**
- これは nightly の Rust を必要とする unstable な機能です

---

本ページは [Custom Targets (stable)](https://doc.rust-lang.org/stable/rustc/targets/custom.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
