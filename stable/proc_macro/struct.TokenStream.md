---
title: TokenStream
---

# Struct TokenStream

```rust
pub struct TokenStream(/* private fields */);
```

**利用可能バージョン:** Rust 1.15.0 から

`TokenStream` は `proc_macro` クレートが提供する主要な型で、トークンの抽象的なストリーム、つまり一連のトークンツリーを表します。`#[proc_macro]`、`#[proc_macro_attribute]`、`#[proc_macro_derive]` の定義において、入力と出力の両方として使われます。

## 関連関数

### `new()`

```rust
pub fn new() -> TokenStream
```

トークンツリーを1つも含まない、空の `TokenStream` を返します。

**利用可能バージョン:** 1.29.0 から

## メソッド

### `is_empty()`

```rust
pub fn is_empty(&self) -> bool
```

この `TokenStream` が空かどうかを確認します。

**利用可能バージョン:** 1.29.0 から

### `expand_expr()`

```rust
pub fn expand_expr(&self) -> Result<TokenStream, ExpandError>
```

🔬 **nightly 限定の実験的 API** (`proc_macro_expand` [#90765](https://github.com/rust-lang/rust/issues/90765))

この `TokenStream` を式として解析し、その中のマクロを展開しようとします。展開後の `TokenStream` を返します。

現時点では、リテラルに展開される式のみが成功しますが、これは将来緩和される可能性があります。

**注意:** エラー条件下では、`expand_expr` はマクロを展開しないままにしたり、エラーを報告してコンパイルを失敗させたり、`Err(..)` を返したりすることがあります。エラー条件ごとの具体的な挙動は未規定であり、将来変わる可能性があります。

## トレイト実装

### 基本トレイト

**Clone:** `TokenStream` の複製機能を提供します。

**Debug:** デバッグに便利な形でトークンを表示します。

**Default:** その型の「デフォルト値」を返します（`TokenStream::new()` と同等）。

**Display:** トークンストリームを、（スパンを除き）損失なく同じトークンストリームへ戻せる文字列として表示します。ただし `Delimiter::None` の区切りを持つ `TokenTree::Group` や、負の数値リテラルは例外です。

**Display に関する注意:** 出力の正確な形式は変更される可能性があります。proc マクロの実装において、この出力文字列への単純な部分文字列マッチングを使わないでください。代わりに `TokenTree` レベルで作業してください（`TokenTree::Ident`、`TokenTree::Punct`、`TokenTree::Literal` に対するマッチングなど）。

### イテレータ・コレクション系トレイト

**Extend<TokenTree>:** イテレータから得られるトークンツリーでコレクションを拡張します。

**Extend<TokenStream>:** トークンストリーム群を、1つのストリームへフラット化しつつ拡張します。

**Extend<Group>、Extend<Ident>、Extend<Literal>、Extend<Punct>:** 個別のトークン種別に特化した拡張の実装です。

**FromIterator<TokenTree>:** 複数のトークンツリーを1つのストリームへ集約します。

**FromIterator<TokenStream>:** 複数のトークンストリームから得られるトークンツリーを1つのストリームへ集約する「フラット化」操作です。

**From<TokenTree>:** 単一のトークンツリーを含むトークンストリームを作成します。

**FromStr:** 文字列をトークンへ分割し、トークンストリームとして解析しようとします。区切り記号の不整合や不正な文字によって失敗する場合があります。解析されたトークンはすべて `Span::call_site()` のスパンを持ちます。

```rust
type Err = LexError;
```

**注意:** 一部のエラーは、`LexError` を返す代わりにパニックを引き起こすことがあります。この挙動は将来のバージョンで変わる可能性があります。

**IntoIterator:** `TokenStream` を `TokenTree` 値に対するイテレータへ変換します。

```rust
type Item = TokenTree;
type IntoIter = IntoIter;
```

**ToTokens:** 🔬 nightly 限定の実験的 API (`proc_macro_totokens` [#130977](https://github.com/rust-lang/rust/issues/130977))

トークンストリームへの変換を実装し、次のメソッドを提供します。
- `to_tokens(&self, tokens: &mut TokenStream)` - 与えられた `TokenStream` へ自身を書き込む
- `into_token_stream(self) -> TokenStream` - 自身を直接 `TokenStream` へ変換する
- `to_token_stream(&self) -> TokenStream` - 自身を直接 `TokenStream` へ変換する

### 否定トレイト

**!Send:** `TokenStream` は `Send` を実装していません。

**!Sync:** `TokenStream` は `Sync` を実装していません。

## Auto Trait の実装

- **Freeze**
- **RefUnwindSafe**
- **Unpin**
- **UnsafeUnpin**
- **UnwindSafe**

## Blanket 実装

すべての型に対して利用可能な標準トレイト実装:
- **Any**
- **Borrow\<T\>** と **BorrowMut\<T\>**
- **CloneToUninit**
- **From\<T\>** と **Into\<U\>**
- **ToOwned** と **ToString**
- **TryFrom\<U\>** と **TryInto\<U\>**

---

本ページは [`proc_macro::TokenStream` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.TokenStream.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
