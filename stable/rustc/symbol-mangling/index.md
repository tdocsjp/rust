---
title: シンボルマングリング
---

# シンボルマングリング（要約）

**目的**: シンボルマングリングは、コード生成の際にシンボルの一意な名前をエンコードし、リンカが名前とそれに対応する定義を正しく関連付けられるようにします。

**マングリング方式**:
- **v0**: 現在の安定したマングリング方式（デフォルト）
- **legacy**: 後方互換性のために nightly ビルドで使える、より古い方式

**v0 の選択**: コンパイラフラグ `-C symbol-mangling-version=v0` で明示的に v0 方式を選べます（すでにデフォルトですが）。

**制御オプション**:
- `#[no_mangle]` — 特定の項目の名前マングリングを無効にする
- `#[export_name]` — エクスポートする正確な名前を指定する
- `#[link_name]` — 外部シンボル参照をカスタマイズする

**デマングル**: `gdb`、`lldb`、`rustc-demangle` クレートのようなツールで、マングルされた名前を読みやすい形にデコードできます（例: `_RNvCskwGfYPst2Cb_3foo16example_function` → `foo::example_function`）。

v0 形式の正確な文法仕様（詳細なエンコーディング規則）については、[v0 Symbol Format](https://doc.rust-lang.org/stable/rustc/symbol-mangling/v0.html) を参照してください。

---

本ページは [Symbol Mangling (stable)](https://doc.rust-lang.org/stable/rustc/symbol-mangling/index.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
