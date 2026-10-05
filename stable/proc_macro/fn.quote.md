---
title: quote (fn)
---

# Function quote

```rust
pub fn quote(stream: TokenStream) -> TokenStream
```

🔬 **nightly 限定の実験的 API** (`proc_macro_quote` [#54722](https://github.com/rust-lang/rust/issues/54722))

`TokenStream` を `TokenStream` へクォートします。これは `quote!()` proc マクロの実際の実装です。

コンパイラの `register_builtin_macros` によって読み込まれます。

---

本ページは [`proc_macro::quote` (stable, 関数)](https://doc.rust-lang.org/stable/proc_macro/fn.quote.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
