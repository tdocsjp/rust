---
title: Rust ドキュメント
---

# Rust ドキュメント

[Rust プロジェクト](https://www.rust-lang.org)が提供するドキュメントの概要です。このページには各種の参考資料へのリンクがまとまっており、そのほとんどはオフラインでも読めます（`rustup doc` で開いた場合）。これらの資料は多くが「本（book）」の形をとっており、まとめて「Rust 本棚（The Rust Bookshelf）」と呼んでいます。大きなものもあれば、小さなものもあります。

ここに挙げた本はすべて Rust Organization が管理していますが、非公式のドキュメント資料もあわせて掲載しています。

標準ライブラリのリファレンスを探しているだけなら、こちらです: [Rust API ドキュメント](/stable/std/)

## Rust を学ぶ

Rust を学びたい人向けのセクションです。ここに挙げた資料はいずれも、何らかのプログラミング経験があることを前提としていますが、特定の言語の知識は前提としていません。

### The Rust Programming Language

親しみを込めて「the book（あの本）」と呼ばれる [The Rust Programming Language](https://doc.rust-lang.org/stable/book/index.html) は、この言語を第一原理から概観させてくれます。読み進めながらいくつかのプロジェクトを作り、読み終えたときには言語の使い方をしっかり把握できているはずです。

### Rust By Example

言語について数百ページ読むのが好みでないなら、[Rust By Example](https://doc.rust-lang.org/stable/rust-by-example/index.html) が向いています。RBE は言葉を多く使わず、大量のコードを見せていきます。練習問題もついています。

### Rustlings

[Rustlings](https://github.com/rust-lang/rustlings) は Rust ツールチェインのダウンロードとセットアップを案内し、そのうえで Rust のコーディング課題の解き方を教えてくれる対話的なツールを提供します。

### Rust Playground

[Rust Playground](https://play.rust-lang.org) は、短いコードを試したり共有したり、人気のクレートを試してみるのに最適な場所です。

## Rust を使う

言語に慣れてきたら、こちらの資料が実際に使っていく助けになります。

### 標準ライブラリ

Rust の標準ライブラリには[充実した API ドキュメント](/stable/std/)があり、さまざまな機能の使い方の説明と、目的別のサンプルコードが載っています。コード例はホバーすると「Run」ボタンが現れ、Playground でそのサンプルを開けます。

### 自分のプロジェクトのドキュメント

クレートで作業しているときはいつでも、`cargo doc --open` を実行すれば、自分のプロジェクト*と*その依存すべてについて、正しいバージョンのドキュメントが生成され、ブラウザで開かれます。`--document-private-items` フラグを付けると、`pub` が付いていない項目も表示されます。

### Rust のバージョン履歴

[リリースノート](https://doc.rust-lang.org/stable/releases.html)には、Rust ツールチェインと言語の変更履歴が記載されています。

[エディションガイド](https://doc.rust-lang.org/stable/edition-guide/index.html)は、Rust のエディションとその違いを説明しています。最新のツールチェインは、過去のすべてのエディションをサポートしています。

### The `rustc` Book

[The `rustc` Book](https://doc.rust-lang.org/stable/rustc/index.html) は Rust コンパイラ `rustc` について説明しています。

### The Cargo Book

[The Cargo Book](https://doc.rust-lang.org/stable/cargo/index.html) は、Rust のビルドツールであり依存管理ツールでもある Cargo のガイドです。

### The Rustdoc Book

[The Rustdoc Book](https://doc.rust-lang.org/stable/rustdoc/index.html) は、ドキュメント生成ツール `rustdoc` について説明しています。

### The Clippy Book

[The Clippy Book](https://doc.rust-lang.org/stable/clippy/index.html) は、静的解析ツール Clippy について説明しています。

### エラーコード一覧

Rust のエラーの多くにはエラーコードが付いており、そうしたエラーについてはコンパイラに詳しい診断を求められます（`rustc --explain`）。お好みであれば、こちらでも読めます: [rustc エラーコード](https://doc.rust-lang.org/stable/error_codes/index.html)

## Rust を極める

言語に十分慣れてきたら、こうした上級者向けの資料が役に立つかもしれません。

### The Reference

[The Reference](https://doc.rust-lang.org/stable/reference/index.html) は形式的な仕様書ではありませんが、the book よりも詳細で網羅的です。

### The Style Guide

[The Rust Style Guide](https://doc.rust-lang.org/stable/style-guide/index.html) は、Rust コードの標準的な書式を定めています。ほとんどの開発者は `cargo fmt` 経由で `rustfmt` を呼び出し、コードを自動整形しています（その結果はこのスタイルガイドに一致します）。

### The Rustonomicon

[The Rustonomicon](https://doc.rust-lang.org/stable/nomicon/index.html) は、unsafe な Rust という闇の魔術への手引き書です。「the 'nomicon」と呼ばれることもあります。

### The Unstable Book

[The Unstable Book](https://doc.rust-lang.org/stable/unstable-book/index.html) には、unstable な機能のドキュメントが載っています。

### The `rustc` Development Guide

[The `rustc-dev-guide`](https://rustc-dev-guide.rust-lang.org/) は、コンパイラの仕組みと、その開発に貢献する方法を解説しています。Rust コンパイラをソースからビルドしたり改変したい場合（たとえば標準的でないターゲット向けにしたい場合など）に役立ちます。

## 特定分野の Rust

特定の分野で Rust を使う場合は、その領域に合わせた次の資料も検討してください。

### 組込みシステム

ベアメタルや組込み Linux 向けの開発では、[Embedded Working Group](https://github.com/rust-embedded) が管理する次の資料が役に立つでしょう。

#### The Embedded Rust Book

[The Embedded Rust Book](https://doc.rust-lang.org/stable/embedded-book/index.html) は、組込み開発と Rust の両方には慣れているが、組込み開発で Rust を使ったことはない開発者を対象としています。

---

本ページは [Rust Documentation (stable)](https://doc.rust-lang.org/stable/) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
