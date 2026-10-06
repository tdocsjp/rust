---
title: proc_macro
---

# proc_macro

## クレートの説明

マクロの作者が新しいマクロを定義する際に使う、サポートライブラリです。

このライブラリは標準ディストリビューションによって提供され、関数型マクロ `#[proc_macro]`、マクロ属性 `#[proc_macro_attribute]`、カスタム derive 属性 `#[proc_macro_derive]` といった、手続き的に定義されるマクロ定義のインタフェースで使われる型を提供します。

詳しくは[the book](https://doc.rust-lang.org/stable/book/ch19-06-macros.html#procedural-macros-for-generating-code-from-attributes)を参照してください。

## モジュール

- [token_stream](https://doc.rust-lang.org/stable/proc_macro/token_stream/index.html) — `TokenStream` 型のための、イテレータなどの公開実装詳細。
- [tracked](https://doc.rust-lang.org/stable/proc_macro/tracked/index.html) — ビルド依存情報に環境状態を追加するための機能。*実験的*

## マクロ

- [quote](https://doc.rust-lang.org/stable/proc_macro/macro.quote.html) — `quote!(..)` は任意のトークンを受け取り、その入力を表す `TokenStream` に展開されます。たとえば `quote!(a + b)` は、評価されると `TokenStream` である `[Ident("a"), Punct('+', Alone), Ident("b")]` を構築する式を生成します。*実験的*

## 構造体

- [Group](https://doc.rust-lang.org/stable/proc_macro/struct.Group.html) — 区切られたトークンストリーム。
- [Ident](https://doc.rust-lang.org/stable/proc_macro/struct.Ident.html) — 識別子 (`ident`)。
- [LexError](https://doc.rust-lang.org/stable/proc_macro/struct.LexError.html) — `TokenStream::from_str` から返されるエラー。
- [Literal](https://doc.rust-lang.org/stable/proc_macro/struct.Literal.html) — 文字列リテラル (`"hello"`)、バイト文字列 (`b"hello"`)、C 文字列 (`c"hello"`)、文字 (`'a'`)、バイト文字 (`b'a'`)、接尾辞の有無を問わない整数または浮動小数点数 (`1`、`1u8`、`2.3`、`2.3f32`)。`true` や `false` のような真偽値リテラルはここには含まれず、`Ident` として扱われます。
- [Punct](https://doc.rust-lang.org/stable/proc_macro/struct.Punct.html) — `Punct` は `+`、`-`、`#` のような単一の句読点文字です。
- [Span](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html) — ソースコードの領域と、それに付随するマクロ展開情報。
- [TokenStream](https://doc.rust-lang.org/stable/proc_macro/struct.TokenStream.html) — このクレートが提供する主要な型で、トークンの抽象的なストリーム、より正確には一連のトークンツリーを表します。この型は、それらのトークンツリーを反復するためのインタフェースと、逆に複数のトークンツリーを1つのストリームへ集約するためのインタフェースを提供します。
- [Diagnostic](https://doc.rust-lang.org/stable/proc_macro/struct.Diagnostic.html) — 診断メッセージと、それに関連する子メッセージを表す構造体。*実験的*
- [ExpandError](https://doc.rust-lang.org/stable/proc_macro/struct.ExpandError.html) — `TokenStream::expand_expr` から返されるエラー。*実験的*

## 列挙型

- [Delimiter](https://doc.rust-lang.org/stable/proc_macro/enum.Delimiter.html) — 一連のトークンツリーがどのように区切られているかを表します。
- [Spacing](https://doc.rust-lang.org/stable/proc_macro/enum.Spacing.html) — `Punct` トークンが次のトークンと結合して複数文字の演算子を形成できるかどうかを示します。
- [TokenTree](https://doc.rust-lang.org/stable/proc_macro/enum.TokenTree.html) — 単一のトークン、または区切られた一連のトークンツリー（例: `[1, (), ..]`）。
- [ConversionErrorKind](https://doc.rust-lang.org/stable/proc_macro/enum.ConversionErrorKind.html) — リテラルのエスケープ解除後の値を取得しようとしたときに返されるエラー。*実験的*
- [EscapeError](https://doc.rust-lang.org/stable/proc_macro/enum.EscapeError.html) — 主に不正なエスケープシーケンスに関連するものですが、その他いくつかの問題も含みます。*実験的*
- [Level](https://doc.rust-lang.org/stable/proc_macro/enum.Level.html) — 診断レベルを表す列挙型。*実験的*

## トレイト

- [MultiSpan](https://doc.rust-lang.org/stable/proc_macro/trait.MultiSpan.html) — `Span` の集合に変換できる型によって実装されるトレイト。*実験的*
- [ToTokens](https://doc.rust-lang.org/stable/proc_macro/trait.ToTokens.html) — [`quote!`](https://doc.rust-lang.org/stable/proc_macro/macro.quote.html) の呼び出しの中に埋め込める型。*実験的*

## 関数

- [is_available](https://doc.rust-lang.org/stable/proc_macro/fn.is_available.html) — proc_macro が現在実行中のプログラムからアクセス可能になっているかどうかを判定します。
- [quote](https://doc.rust-lang.org/stable/proc_macro/fn.quote.html) — `TokenStream` を `TokenStream` へクォートします。これは `quote!()` proc マクロの実際の実装です。*実験的*
- [quote_span](https://doc.rust-lang.org/stable/proc_macro/fn.quote_span.html) — `Span` を `TokenStream` へクォートします。これはカスタムのクォーターを実装するために必要です。*実験的*

---

本ページは [`proc_macro` (stable)](https://doc.rust-lang.org/stable/proc_macro/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
