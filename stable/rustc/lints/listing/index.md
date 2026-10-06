---
title: Lint 一覧
---

# Lint 一覧（概要）

rustc の個別 lint は、デフォルトのレベルに応じて3つのページに分かれています。

- [許可レベルの lint](allowed-by-default.html)（デフォルトで `allow`、約124件） — 既定では無効だが、必要に応じて `-W`/`#[warn(...)]` などで有効化できる lint
- [警告レベルの lint](warn-by-default.html)（デフォルトで `warn`、約282件） — デフォルトで警告を出す lint
- [禁止レベルの lint](deny-by-default.html)（デフォルトで `deny`、約98件） — デフォルトでエラーになる lint

合計500件以上の個別 lint があり、それぞれに名前・説明・コード例が付いています。この量を考えると、個々の lint の説明を1件ずつ翻訳するのではなく、ここでは全体の構成だけを示します。特定の lint（たとえば `unused_variables` や `dead_code`）の詳しい説明が必要な場合は、[doc.rust-lang.org](https://doc.rust-lang.org/stable/rustc/lints/listing/index.html) の該当ページを参照してください。

lint のレベルの設定方法や優先順位については[Lint レベル](../levels.md)を、関連する lint をまとめて扱う方法については[Lint グループ](../groups.md)を参照してください。

---

本ページは [Lint Listing (stable)](https://doc.rust-lang.org/stable/rustc/lints/listing/index.html) の概要の非公式日本語訳です。個別 lint の一覧・説明は [doc.rust-lang.org](https://doc.rust-lang.org/stable/rustc/lints/listing/index.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
