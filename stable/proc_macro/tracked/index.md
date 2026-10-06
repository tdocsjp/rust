---
title: tracked
---

# Module tracked

🔬 **nightly 限定の実験的 API** (`proc_macro_tracked_path` [#99515](https://github.com/rust-lang/rust/issues/99515))

ビルド依存情報に環境状態を追加するための機能です。

## 関数

### [env_var](./fn.env_var.md) — 実験的

環境変数を取得し、それをビルド依存情報に追加します。コンパイラを実行するビルドシステムは、その変数がコンパイル中にアクセスされたことを把握し、その変数の値が変わったときにビルドを再実行できるようになります。依存関係の追跡を除けば、この関数は標準ライブラリの `env::var` と同等であるべきですが、引数が UTF-8 でなければならない点が異なります。

### [path](./fn.path.md) — 実験的

ファイルやディレクトリを明示的に追跡します。

---

本ページは [`proc_macro::tracked` (stable)](https://doc.rust-lang.org/stable/proc_macro/tracked/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
