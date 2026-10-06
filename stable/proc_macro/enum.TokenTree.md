---
title: TokenTree
---

# Enum TokenTree

単一のトークン、または区切られた一連のトークンツリー（例: `[1, (), ..]`）を表します。

## 定義

```rust
pub enum TokenTree {
    Group(Group),
    Ident(Ident),
    Punct(Punct),
    Literal(Literal),
}
```

## バリアント

- **Group(Group)** - 区切り記号で囲まれたトークンストリーム
- **Ident(Ident)** - 識別子
- **Punct(Punct)** - 単一の句読点文字（`+`、`,`、`$` など）
- **Literal(Literal)** - リテラルの文字 (`'a'`)、文字列 (`"hello"`)、数値 (`2.3`) など

## メソッド

### `span(&self) -> Span`

このツリーのスパンを返します。内部のトークンまたは区切られたストリームの `span` メソッドに委譲します。

### `set_span(&mut self, span: Span)`

_このトークンだけ_のスパンを設定します。このトークンが `Group` である場合、このメソッドは内部の各トークンのスパンは設定しません。単に各バリアントの `set_span` メソッドに委譲するだけです。

## 主なトレイト実装

- **Clone** - 値の複製を返す
- **Debug** - トークンツリーをデバッグに便利な形で表示する
- **Display** - トークンツリーを、（スパンを除き）損失なく変換できる文字列として表示する
- **From<Group/Ident/Punct/Literal>** - 各バリアントを `TokenTree` へ変換する
- **From<TokenTree> for TokenStream** - 単一のトークンツリーからトークンストリームを作成する
- **ToTokens** - `TokenStream` へ変換する（nightly API）

## Auto Trait

- **!Send** - `Send` ではない
- **!Sync** - `Sync` ではない
- **Freeze、Unpin、UnwindSafe、RefUnwindSafe** - いずれも自動的に実装される

## Display に関する重要な注意

`Display` の実装は、（`Delimiter::None` を持つ `Group` や負の数値リテラルを除き）同じトークンツリーへ損失なく戻せることを意図した出力を生成します。proc マクロの実装において**この出力への部分文字列マッチングを使わないでください**。代わりに、`TokenTree::Ident`、`TokenTree::Punct`、`TokenTree::Literal` に対するマッチングによって `TokenTree` レベルで作業してください。

---

本ページは [`proc_macro::TokenTree` (stable)](https://doc.rust-lang.org/stable/proc_macro/enum.TokenTree.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
