---
title: Linker-plugin ベースの LTO
---

# Linker-plugin ベースの LTO（要約）

## 概要

Linker-plugin ベースの LTO は、リンク時最適化 (LTO) を実際のリンクの段階まで遅らせる機能で、すべてのオブジェクトファイルが同じ LTO モード（thin または fat）を使う LLVM ベースのツールチェインで作られている場合、プログラミング言語の境界を越えた相互手続き最適化を可能にします。

## `-C linker-plugin-lto` フラグ

このフラグで linker-plugin ベースの LTO を有効にします。

- **デフォルトの挙動**: thin LTO を有効にする
- **fat LTO**: 追加で `-C lto=fat` フラグが必要
- **プラグインパスの指定**: `-Clinker-plugin-lto="/path/to/LLVMgold.so"` のように LLVM プラグインを明示的に指定できる

## 主な用途

1. C/C++ の依存としての Rust の staticlib — Rust を静的ライブラリにコンパイルし、C/C++ のコードとリンクする
2. Rust の依存としての C/C++ — 外部の C/C++ ライブラリを Rust のバイナリにリンクする
3. Fortran との相互運用 — LLVM の `flang` でコンパイルした Fortran を Rust とリンクする

## 例

```bash
# linker-plugin LTO で Rust の staticlib をコンパイルする
rustc --crate-type=staticlib -Clinker-plugin-lto -Copt-level=2 ./lib.rs

# 対応する thin LTO で C コードをコンパイルする
clang -c -O2 -flto=thin -o cmain.o ./cmain.c

# LLVM リンカプラグイン対応のリンカ（LLD）でリンクする
clang -flto=thin -fuse-ld=lld -L . -l"rust-lib" -o main ./cmain.o
```

## 重要な要件: LLVM バージョンの一致

関係するすべてのコンパイラが**互換性のある LLVM バージョン**を使う必要があります。ベストプラクティスは、まったく同じ LLVM バージョンを使うことです。`rustc -vV` で LLVM のバージョンを確認できます。

バージョンが一致しないと、通常リンカエラーになります。

---

本ページは [Linker-plugin-based LTO (stable)](https://doc.rust-lang.org/stable/rustc/linker-plugin-lto.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
