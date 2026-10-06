---
title: Ident
---

# Struct Ident

```rust
pub struct Ident(/* private fields */);
```

識別子 (`ident`)。

**利用可能バージョン:** 1.29.0 から

## 関連関数

### `new`

```rust
pub fn new(string: &str, span: Span) -> Ident
```

与えられた `string` と、指定した `span` を持つ新しい `Ident` を作成します。`string` 引数は、言語が許容する正当な識別子（`self` や `fn` のようなキーワードを含む）でなければなりません。そうでない場合、この関数はパニックします。

構築される識別子は NFC 正規化されます。詳しくは [Reference](https://doc.rust-lang.org/nightly/reference/identifiers.html#r-ident.normalization) を参照してください。

`span` は現在の rustc では、この識別子のハイジーン情報を設定するものであることに注意してください。

現時点では、`Span::call_site()` は明示的に「呼び出し元 (call-site)」のハイジーンを選択します。つまり、このスパンで作成された識別子は、まるでマクロ呼び出しの場所に直接書かれたかのように解決され、マクロ呼び出し元にある他のコードもそれらを参照できます。

これに対し、将来の `Span::def_site()` のようなスパンは「定義元 (definition-site)」のハイジーンを選択できるようにします。つまり、そのスパンで作成された識別子はマクロ定義の場所で解決され、マクロ呼び出し元にある他のコードはそれらを参照できません。

ハイジーンの重要性から、このコンストラクタは他のトークンとは異なり、構築時に `Span` を指定することを要求します。

**利用可能バージョン:** 1.29.0 から

### `new_raw`

```rust
pub fn new_raw(string: &str, span: Span) -> Ident
```

`Ident::new` と同様ですが、raw 識別子 (`r#ident`) を作成します。`string` 引数は、言語が許容する正当な識別子（`fn` のようなキーワードを含む）でなければなりません。パスセグメントで使用可能なキーワード（`self`、`super` など）はサポートされておらず、パニックを引き起こします。

**利用可能バージョン:** 1.47.0 から

## メソッド

### `span`

```rust
pub fn span(&self) -> Span
```

この `Ident` のスパンを返します。[`to_string`](https://doc.rust-lang.org/stable/alloc/string/trait.ToString.html#tymethod.to_string) が返す文字列全体を包含します。

**利用可能バージョン:** 1.29.0 から

### `set_span`

```rust
pub fn set_span(&mut self, span: Span)
```

この `Ident` のスパンを設定し、そのハイジーンのコンテキストを変更することがあります。

**利用可能バージョン:** 1.29.0 から

## トレイト実装

### Clone

```rust
impl Clone for Ident
```

値の複製を返します。

### Debug

```rust
impl Debug for Ident
```

与えられたフォーマッタを使って値を整形します。

### Display

```rust
impl Display for Ident
```

識別子を、損失なく同じ識別子へ戻せるはずの文字列として表示します。

### Extend<Ident> for TokenStream

```rust
impl Extend<Ident> for TokenStream
```

イテレータの内容でコレクションを拡張します。

**利用可能バージョン:** 1.92.0 から

### From<Ident> for TokenTree

```rust
impl From<Ident> for TokenTree
```

入力された型からこの型へ変換します。

**利用可能バージョン:** 1.29.0 から

### ToTokens

```rust
impl ToTokens for Ident
```

`Ident` をトークンストリームへ変換するメソッドを提供します。
- `to_tokens(&self, tokens: &mut TokenStream)` - 与えられた `TokenStream` へ `self` を書き込む
- `to_token_stream(&self) -> TokenStream` - `self` を直接 `TokenStream` オブジェクトへ変換する
- `into_token_stream(self) -> TokenStream` - `self` を直接 `TokenStream` オブジェクトへ変換する

**注意:** これらは実験的 API です (`proc_macro_totokens` [#130977](https://github.com/rust-lang/rust/issues/130977))

## Auto Trait の実装

- **!Send** - Send を実装しない
- **!Sync** - Sync を実装しない
- **Freeze**
- **RefUnwindSafe**
- **Unpin**
- **UnsafeUnpin**
- **UnwindSafe**

---

本ページは [`proc_macro::Ident` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.Ident.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
