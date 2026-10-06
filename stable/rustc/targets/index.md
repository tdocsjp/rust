---
title: ターゲット
---

# ターゲット（要約）

**ターゲット**は、`rustc` がコードをコンパイルできる、ありうるアーキテクチャを表します。`rustc` はデフォルトでクロスコンパイラであるため、ターゲットを指定することで、どのアーキテクチャ向けにも、どのコンパイラからでもビルドできます。

## ターゲットの使い方

特定のターゲット向けにコンパイルするには `--target` フラグを使います。

```bash
rustc src/main.rs --target=wasm32-unknown-unknown
```

## ターゲットフィーチャー

基本のターゲットアーキテクチャに加えて、`-C target-feature=val` フラグで CPU 固有の命令セットを有効にできます。例:

- ベクトル命令（AVX）
- ビット操作（BMI）
- 暗号化（AES）

**注意:** このフラグは一般に unsafe とみなされます。

---

本ページは [Targets (stable)](https://doc.rust-lang.org/stable/rustc/targets/index.html) の要約の非公式日本語訳です。ターゲットタプルの構成要素や Tier（1/2/3）制度については [Platform Support](../platform-support.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
