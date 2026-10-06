---
title: quote (macro)
---

# Macro quote

```rust
pub macro quote($($t:tt)*) {
    ...
}
```

🔬 **nightly 限定の実験的 API** (`proc_macro_quote` [#54722](https://github.com/rust-lang/rust/issues/54722))

`quote!(..)` は任意のトークンを受け取り、その入力を表す `TokenStream` に展開されます。たとえば `quote!(a + b)` は、評価されると `TokenStream` である `[Ident("a"), Punct('+', Alone), Ident("b")]` を構築する式を生成します。

アンクォートは `$` で行い、その直後の単一の識別子をアンクォートされた項として取り込む形で動作します。`$` 自体をクォートするには `$$` を使います。

---

本ページは [`proc_macro::quote` (stable, マクロ)](https://doc.rust-lang.org/stable/proc_macro/macro.quote.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
