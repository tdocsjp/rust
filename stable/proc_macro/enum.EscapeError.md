---
title: EscapeError
---

# Enum EscapeError

🔬 **nightly 限定の実験的 API** (`proc_macro_value` [#136652](https://github.com/rust-lang/rust/issues/136652))

主に不正なエスケープシーケンスに関連するものですが、その他いくつかの問題も含む、非網羅的な enum です。

## 定義

```rust
#[non_exhaustive]
pub enum EscapeError {
    ZeroChars,
    MoreThanOneChar,
    LoneSlash,
    InvalidEscape,
    BareCarriageReturn,
    BareCarriageReturnInRawString,
    EscapeOnlyChar,
    TooShortHexEscape,
    InvalidCharInHexEscape,
    OutOfRangeHexEscape,
    NoBraceInUnicodeEscape,
    InvalidCharInUnicodeEscape,
    EmptyUnicodeEscape,
    UnclosedUnicodeEscape,
    LeadingUnderscoreUnicodeEscape,
    OverlongUnicodeEscape,
    LoneSurrogateUnicodeEscape,
    OutOfRangeUnicodeEscape,
    UnicodeEscapeInByte,
    NonAsciiCharInByte,
    NulInCStr,
    UnskippedWhitespaceWarning,
    MultipleSkippedLinesWarning,
}
```

## バリアント

| バリアント | 説明 |
|---------|-------------|
| **ZeroChars** | 1文字を期待していたが、0文字だった。 |
| **MoreThanOneChar** | 1文字を期待していたが、2文字以上あった。 |
| **LoneSlash** | 続きのない、エスケープされた `\` 文字。 |
| **InvalidEscape** | 不正なエスケープ文字（例: `\z`）。 |
| **BareCarriageReturn** | 生の `\r` が検出された。 |
| **BareCarriageReturnInRawString** | raw 文字列中に生の `\r` が検出された。 |
| **EscapeOnlyChar** | エスケープされているべきなのにされていない文字（例: 生の `\t`）。 |
| **TooShortHexEscape** | 数値による文字エスケープが短すぎる（例: `\x1`）。 |
| **InvalidCharInHexEscape** | 数値エスケープ中に不正な文字がある（例: `\xz`）。 |
| **OutOfRangeHexEscape** | 数値エスケープ中の文字コードが ASCII でない（例: `\xFF`）。 |
| **NoBraceInUnicodeEscape** | `\u` の後に `{` が続かない。 |
| **InvalidCharInUnicodeEscape** | `\u{..}` 内に16進数以外の値がある。 |
| **EmptyUnicodeEscape** | `\u{}`（空の波括弧）。 |
| **UnclosedUnicodeEscape** | `\u{..}` に閉じ括弧がない。例: `\u{12`。 |
| **LeadingUnderscoreUnicodeEscape** | `\u{_12}`（先頭にアンダースコア）。 |
| **OverlongUnicodeEscape** | `\u{..}` 内が6文字を超える。例: `\u{10FFFF_FF}`。 |
| **LoneSurrogateUnicodeEscape** | 範囲内だが不正な Unicode 文字コード。例: `\u{DFFF}`。 |
| **OutOfRangeUnicodeEscape** | 範囲外の Unicode 文字コード。例: `\u{FFFFFF}`。 |
| **UnicodeEscapeInByte** | バイトリテラル中の Unicode エスケープコード。 |
| **NonAsciiCharInByte** | バイトリテラル、バイト文字列リテラル、raw バイト文字列リテラル中の非 ASCII 文字。 |
| **NulInCStr** | C 文字列リテラル中の `\0`。 |
| **UnskippedWhitespaceWarning** | `\` で終わる行の後、次の行にスキップされていない空白文字がある。 |
| **MultipleSkippedLinesWarning** | `\` で終わる行の後、複数行がスキップされている。 |

## トレイト実装

- **Debug**: デバッグ用にエラーを整形する
- **Display**: ユーザー向け表示用にエラーを整形する
- **Eq**: 等価性の比較
- **PartialEq**: 部分的な等価性の比較
- **Error**: 標準の Rust エラートレイト
- **StructuralPartialEq**: 構造的等価性のためのマーカートレイト

## Auto Trait

- Send、Sync、Unpin、UnwindSafe、RefUnwindSafe、Freeze、UnsafeUnpin

## 補足

- この enum には `#[non_exhaustive]` が付いており、将来のバージョンで新しいバリアントが追加される可能性があります。
- パターンマッチングの際は、将来のバリアントを扱うためにワイルドカードアーム (`_`) を含める必要があります。
- すべてのバリアントは、`proc_macro_value` フィーチャーフラグ (#136652) を必要とする、nightly 限定の実験的機能です。

---

本ページは [`proc_macro::EscapeError` (stable)](https://doc.rust-lang.org/stable/proc_macro/enum.EscapeError.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
