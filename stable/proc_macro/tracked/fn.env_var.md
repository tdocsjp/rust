---
title: env_var
---

# Function env_var

```rust
pub fn env_var<K: AsRef<OsStr> + AsRef<str>>(key: K) -> Result<String, VarError>
```

🔬 **nightly 限定の実験的 API** (`proc_macro_tracked_env` [#99515](https://github.com/rust-lang/rust/issues/99515))

環境変数を取得し、それをビルド依存情報に追加します。コンパイラを実行するビルドシステムは、その変数がコンパイル中にアクセスされたことを把握し、その変数の値が変わったときにビルドを再実行できるようになります。依存関係の追跡を除けば、この関数は標準ライブラリの `env::var` と同等であるべきですが、引数が UTF-8 でなければならない点が異なります。

---

本ページは [`proc_macro::tracked::env_var` (stable)](https://doc.rust-lang.org/stable/proc_macro/tracked/fn.env_var.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
