---
title: Literal
---

# Struct Literal

```rust
pub struct Literal(/* private fields */);
```

**利用可能バージョン:** 1.29.0 から（Copy 可能）

文字列リテラル (`"hello"`)、バイト文字列 (`b"hello"`)、C 文字列 (`c"hello"`)、文字 (`'a'`)、バイト文字 (`b'a'`)、接尾辞の有無を問わない整数または浮動小数点数 (`1`、`1u8`、`2.3`、`2.3f32`)。`true` や `false` のような真偽値リテラルはここには含まれず、代わりに `Ident` として表現されます。

## 関連関数（コンストラクタ）

### 整数リテラル - 接尾辞付き

次の関数は `1u8` のような、型の接尾辞付き整数リテラルを作成します。

- `pub fn u8_suffixed(n: u8) -> Literal`
- `pub fn u16_suffixed(n: u16) -> Literal`
- `pub fn u32_suffixed(n: u32) -> Literal`
- `pub fn u64_suffixed(n: u64) -> Literal`
- `pub fn u128_suffixed(n: u128) -> Literal`
- `pub fn usize_suffixed(n: usize) -> Literal`
- `pub fn i8_suffixed(n: i8) -> Literal`
- `pub fn i16_suffixed(n: i16) -> Literal`
- `pub fn i32_suffixed(n: i32) -> Literal`
- `pub fn i64_suffixed(n: i64) -> Literal`
- `pub fn i128_suffixed(n: i128) -> Literal`
- `pub fn isize_suffixed(n: isize) -> Literal`

指定した値を持つ、新しい接尾辞付き整数リテラルを作成します。`1u32` のように、整数値がトークンの最初の部分になり、整数型が末尾に接尾辞として付きます。負の数から作成されたリテラルは、`TokenStream` や文字列を経由したラウンドトリップを経ると、そのまま残らず2つのトークン（`-` と正のリテラル）に分かれることがあります。

これらのメソッドで作成されたリテラルはデフォルトで `Span::call_site()` のスパンを持ち、`set_span()` で設定できます。

### 整数リテラル - 接尾辞なし

次の関数は接尾辞のない整数リテラルを作成します。

- `pub fn u8_unsuffixed(n: u8) -> Literal`
- `pub fn u16_unsuffixed(n: u16) -> Literal`
- `pub fn u32_unsuffixed(n: u32) -> Literal`
- `pub fn u64_unsuffixed(n: u64) -> Literal`
- `pub fn u128_unsuffixed(n: u128) -> Literal`
- `pub fn usize_unsuffixed(n: usize) -> Literal`
- `pub fn i8_unsuffixed(n: i8) -> Literal`
- `pub fn i16_unsuffixed(n: i16) -> Literal`
- `pub fn i32_unsuffixed(n: i32) -> Literal`
- `pub fn i64_unsuffixed(n: i64) -> Literal`
- `pub fn i128_unsuffixed(n: i128) -> Literal`
- `pub fn isize_unsuffixed(n: isize) -> Literal`

指定した値を持つ、新しい接尾辞なし整数リテラルを作成します。`1` のように、整数値がトークンの最初の部分になります。このトークンには接尾辞が指定されないため、`Literal::i8_unsuffixed(1)` のような呼び出しは `Literal::u32_unsuffixed(1)` と同等になります。負の数から作成されたリテラルは、`TokenStream` や文字列を経由したラウンドトリップを経ると、そのまま残らず2つのトークンに分かれることがあります。

### 浮動小数点リテラル

**接尾辞なし:**
- `pub fn f32_unsuffixed(n: f32) -> Literal`
- `pub fn f64_unsuffixed(n: f64) -> Literal`

**接尾辞付き:**
- `pub fn f32_suffixed(n: f32) -> Literal`
- `pub fn f64_suffixed(n: f64) -> Literal`

浮動小数点リテラルを作成します。接尾辞なしの版は浮動小数点値をそのままトークンに埋め込みますが接尾辞は使われないため、後にコンパイラによって `f64` と推論される場合があります。接尾辞付きの版は `1.0f32` や `1.0f64` のようなリテラルを作成し、型は常に指定した型に推論されます。

**パニック:** これらの関数は、指定した浮動小数点数が有限であることを要求します。無限大や NaN が渡された場合、この関数はパニックします。

### 文字列・文字リテラル

- `pub fn string(string: &str) -> Literal` - 文字列リテラル
- `pub fn character(ch: char) -> Literal` - 文字リテラル
- `pub fn byte_character(byte: u8) -> Literal` - バイト文字リテラル（1.79.0 から安定化）
- `pub fn byte_string(bytes: &[u8]) -> Literal` - バイト文字列リテラル
- `pub fn c_string(string: &CStr) -> Literal` - C 文字列リテラル（1.79.0 から安定化）

## メソッド

### スパンの管理

```rust
pub fn span(&self) -> Span
```
このリテラルを包含するスパンを返します。

```rust
pub fn set_span(&mut self, span: Span)
```
このリテラルに紐づくスパンを設定します。

```rust
pub fn subspan<R: RangeBounds<usize>>(&self, range: R) -> Option<Span>
```
🔬 **nightly 限定の実験的 API** (`proc_macro_span`)

`range` で指定したソースバイトの範囲だけを含む、`self.span()` の部分集合である `Span` を返します。切り出そうとしたスパンが `self` の範囲外である場合は `None` を返します。

### 値の取得メソッド

🔬 **nightly 限定の実験的 API** (`proc_macro_value`)

これらのメソッドは、リテラルからエスケープ解除後の値を取得します。

- `pub fn byte_character_value(&self) -> Result<u8, ConversionErrorKind>` - バイト文字の値を取得
- `pub fn character_value(&self) -> Result<char, ConversionErrorKind>` - 文字の値を取得
- `pub fn str_value(&self) -> Result<String, ConversionErrorKind>` - 文字列の値を取得
- `pub fn byte_str_value(&self) -> Result<Vec<u8>, ConversionErrorKind>` - バイト文字列の値を取得
- `pub fn cstr_value(&self) -> Result<Vec<u8>, ConversionErrorKind>` - C 文字列の値を取得

**整数値の取得:**
- `pub fn u8_value(&self) -> Result<u8, ConversionErrorKind>`
- `pub fn u16_value(&self) -> Result<u16, ConversionErrorKind>`
- `pub fn u32_value(&self) -> Result<u32, ConversionErrorKind>`
- `pub fn u64_value(&self) -> Result<u64, ConversionErrorKind>`
- `pub fn u128_value(&self) -> Result<u128, ConversionErrorKind>`
- `pub fn i8_value(&self) -> Result<i8, ConversionErrorKind>`
- `pub fn i16_value(&self) -> Result<i16, ConversionErrorKind>`
- `pub fn i32_value(&self) -> Result<i32, ConversionErrorKind>`
- `pub fn i64_value(&self) -> Result<i64, ConversionErrorKind>`
- `pub fn i128_value(&self) -> Result<i128, ConversionErrorKind>`

**浮動小数点値の取得:**
- `pub fn f16_value(&self) -> Result<f16, ConversionErrorKind>`
- `pub fn f32_value(&self) -> Result<f32, ConversionErrorKind>`
- `pub fn f64_value(&self) -> Result<f64, ConversionErrorKind>`

リテラルが指定した種類であるか、オーバーフローしない「マークなし」の整数・浮動小数点数である場合に、エスケープ解除後の値を返します。

## トレイト実装

### Clone
標準的な clone 操作に対応するクローン可能な型。

### Debug
このリテラルのデバッグ整形を実装します。

### Display
リテラルを、損失なく同じリテラルへ戻せるはずの文字列として表示します（浮動小数点リテラルの丸めによる例外はあります）。

### Extend<Literal> for TokenStream
`TokenStream` を `Literal` の値で拡張できます（1.92.0 から安定化）。

### From<Literal> for TokenTree
`Literal` を `TokenTree` へ変換します。

### FromStr
```rust
impl FromStr for Literal
```
文字列化された表現から単一のリテラルを解析します。入力文字列にはリテラルトークンそのもの以外（空白やコメント）を含めてはいけません。

結果として得られるリテラルトークンは `Span::call_site()` のスパンを持ちます。

**関連型:** `type Err = LexError`

**注意:** 一部のエラーは、`LexError` を返す代わりにパニックを引き起こすことがあります。この挙動は将来のバージョンで変わる可能性があります。

### ToTokens
リテラルを `TokenStream` へ変換します（nightly 限定の実験的 API）。

## Auto Trait の実装

- `!Send` - スレッド間で送信できません
- `!Sync` - スレッド間で共有できません
- `Freeze`
- `RefUnwindSafe`
- `Unpin`
- `UnsafeUnpin`
- `UnwindSafe`

## Blanket 実装

基本的なトレイトから導かれる標準トレイト実装:
- `Any`
- `Borrow<T>`
- `BorrowMut<T>`
- `CloneToUninit`
- `From<T>`
- `Into<U>`
- `ToOwned`
- `ToString`
- `TryFrom<U>`
- `TryInto<U>`

---

本ページは [`proc_macro::Literal` (stable)](https://doc.rust-lang.org/stable/proc_macro/struct.Literal.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
