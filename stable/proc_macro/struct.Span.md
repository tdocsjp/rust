---
title: Span
---

# Struct Span

```rust
pub struct Span(/* private fields */);
```

ソースコードの領域と、それに付随するマクロ展開情報を表します。

## 関連関数

### `def_site()`

🔬 **nightly 限定の実験的 API** (`proc_macro_def_site` [#54724](https://github.com/rust-lang/rust/issues/54724))

マクロの定義場所で解決されるスパン。

### `call_site()`

現在の proc マクロの呼び出し場所のスパンです。このスパンで作成された識別子は、まるでマクロ呼び出しの場所に直接書かれたかのように解決され（呼び出し元ハイジーン）、マクロ呼び出し元にある他のコードもそれらを参照できます。

**利用可能バージョン:** 1.29.0 から

### `mixed_site()`

**利用可能バージョン:** 1.45.0 から

`macro_rules` のハイジーンを表すスパンで、時にはマクロ定義場所で解決され（ローカル変数、ラベル、`$crate`）、時にはマクロ呼び出し場所で解決されます（それ以外すべて）。スパンの位置自体は呼び出し元から取られます。

## メソッド

### `parent(&self) -> Option<Span>`

🔬 **nightly 限定の実験的 API** (`proc_macro_span` [#54725](https://github.com/rust-lang/rust/issues/54725))

`self` がそこから生成された、直前のマクロ展開におけるトークンの `Span`（存在する場合）。

### `source(&self) -> Span`

🔬 **nightly 限定の実験的 API** (`proc_macro_span` [#54725](https://github.com/rust-lang/rust/issues/54725))

`self` がそこから生成された元のソースコードに対するスパンです。この `Span` が他のマクロ展開から生成されたものでない場合、戻り値は `*self` と同じです。

### `byte_range(&self) -> Range<usize>`

🔬 **nightly 限定の実験的 API** (`proc_macro_span` [#54725](https://github.com/rust-lang/rust/issues/54725))

このスパンのソースファイル内でのバイト位置の範囲を返します。

### `start(&self) -> Span`

**利用可能バージョン:** 1.88.0 から

このスパンの直前を指す、空のスパンを作成します。

### `end(&self) -> Span`

**利用可能バージョン:** 1.88.0 から

このスパンの直後を指す、空のスパンを作成します。

### `line(&self) -> usize`

**利用可能バージョン:** 1.88.0 から

このスパンが始まるソースファイル中の行番号（1始まり）。

スパンの終端の行番号を得るには `span.end().line()` を使ってください。

### `column(&self) -> usize`

**利用可能バージョン:** 1.88.0 から

このスパンが始まるソースファイル中の列番号（1始まり）。

スパンの終端の列番号を得るには `span.end().column()` を使ってください。

### `file(&self) -> String`

**利用可能バージョン:** 1.88.0 から

このスパンが存在するソースファイルへの、表示目的のパスです。

これは有効なファイルシステム上のパスに対応しているとは限りません。リマップされている場合（例: `"/src/lib.rs"`）や、人為的なパスの場合（例: `"<command line>"`）もあります。

### `local_file(&self) -> Option<PathBuf>`

このスパンが存在するソースファイルへの、ローカルファイルシステム上のパスです。

これはディスク上の実際のパスです。パスのリマップの影響を受けません。

このパスはマクロの出力に埋め込むべきではありません。代わりに `file()` を使ってください。

### `join(&self, other: Span) -> Option<Span>`

🔬 **nightly 限定の実験的 API** (`proc_macro_span` [#54725](https://github.com/rust-lang/rust/issues/54725))

`self` と `other` の両方を包含する新しいスパンを作成します。

`self` と `other` が異なるファイルに属する場合は `None` を返します。

### `resolved_at(&self, other: Span) -> Span`

**利用可能バージョン:** 1.45.0 から

`self` と同じ行・列の情報を持ちながら、`other` の位置であるかのようにシンボルを解決する新しいスパンを作成します。

### `located_at(&self, other: Span) -> Span`

`self` と同じ名前解決の挙動を持ちながら、`other` の行・列の情報を持つ新しいスパンを作成します。

### `eq(&self, other: &Span) -> bool`

🔬 **nightly 限定の実験的 API** (`proc_macro_span` [#54725](https://github.com/rust-lang/rust/issues/54725))

2つのスパンが等しいかどうかを比較します。

### `source_text(&self) -> Option<String>`

**利用可能バージョン:** 1.66.0 から

スパンの背後にあるソーステキストを返します。これは、空白やコメントを含む元のソースコードをそのまま保持します。スパンが実際のソースコードに対応している場合にのみ結果を返します。

**注意:** マクロの観測可能な結果は、トークンのみに依存すべきであり、このソーステキストに依存すべきではありません。この関数の結果は、診断にのみ使うためのベストエフォートです。

### 診断用メソッド

#### `error<T: Into<String>>(self, message: T) -> Diagnostic`

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

スパン `self` の位置に、与えられた `message` を持つ新しい `Diagnostic` を作成します。

#### `warning<T: Into<String>>(self, message: T) -> Diagnostic`

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

スパン `self` の位置に、与えられた `message` を持つ新しい `Diagnostic` を作成します。

#### `note<T: Into<String>>(self, message: T) -> Diagnostic`

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

スパン `self` の位置に、与えられた `message` を持つ新しい `Diagnostic` を作成します。

#### `help<T: Into<String>>(self, message: T) -> Diagnostic`

🔬 **nightly 限定の実験的 API** (`proc_macro_diagnostic` [#54140](https://github.com/rust-lang/rust/issues/54140))

スパン `self` の位置に、与えられた `message` を持つ新しい `Diagnostic` を作成します。

## トレイト実装

### 基本トレイト
- **Clone** (*1.29.0*): 値の複製を返す
- **Copy** (*1.29.0*): コピーのセマンティクスを実装する
- **Debug** (*1.29.0*): デバッグに便利な形でスパンを表示する
- **MultiSpan** (*nightly*): `self` を `Vec<Span>` へ変換する

### マーカートレイト
- **!Send**: `Send` を実装しない
- **!Sync**: `Sync` を実装しない
- **Freeze**: `Freeze` を実装する
- **RefUnwindSafe**: `RefUnwindSafe` を実装する
- **Unpin**: `Unpin` を実装する
- **UnsafeUnpin**: `UnsafeUnpin` を実装する
- **UnwindSafe**: `UnwindSafe` を実装する

## Blanket 実装

次を含む標準トレイト実装が利用可能です。
- Any
- Borrow\<T\>
- BorrowMut\<T\>
- CloneToUninit
- From\<T\>
- Into\<U\>
- ToOwned
- TryFrom\<U\>
- TryInto\<U\>

---

本ページは [`proc_macro::Span` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
