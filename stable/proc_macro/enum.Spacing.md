---
title: Spacing
---

# Enum Spacing

`Punct` トークンが続くトークンと結合して複数文字の演算子を形成できるかどうかを示します。

## 定義

```rust
pub enum Spacing {
    Joint,
    Alone,
}
```

## バリアント

### Joint（1.29.0 以降）

`Punct` トークンが続くトークンと結合して、複数文字の演算子を形成できることを示します。

proc マクロのインタフェースを使って構築されたトークンストリームでは、`Joint` な句読点トークンの後にはどんなトークンでも続けられます。しかし、ソースコードから解析されたトークンストリームでは、コンパイラは次の場合にのみ spacing を `Joint` に設定します。

- `Punct` が空白を挟まずに別の `Punct` の直後に続く場合（例: `+=` や `++` の中の `+` は `Joint`）
- シングルクォート `'` が空白を挟まずに識別子の直後に続く場合（例: `'lifetime` の中の `'` は `Joint`）

このリストは将来拡張される可能性があります。

### Alone（1.29.0 以降）

`Punct` トークンが続くトークンと結合して複数文字の演算子を形成できないことを示します。

`Alone` な句読点トークンの後にはどんなトークンでも続けられます。ソースコードから解析されたトークンストリームでは、コンパイラは `Joint` の条件に当たらないすべての場合に spacing を `Alone` に設定します。例:
- `+ =`、`+ident`、`+()` の中の `+` は `Alone`
- 何も続かないトークンは `Alone` とマークされます

## トレイト実装

- **Clone**: 値の複製を返す
- **Copy**: コピーのセマンティクスを実装する
- **Debug**: フォーマッタを使って値を整形する
- **Eq**: 等価性の比較を実装する
- **PartialEq**: `eq()` と `ne()` メソッドによる部分的な等価性を実装する
- **StructuralPartialEq**: 構造的な部分的等価性のためのマーカートレイト

## Auto Trait の実装

- `Freeze`
- `RefUnwindSafe`
- `Send`
- `Sync`
- `Unpin`
- `UnsafeUnpin`
- `UnwindSafe`

## Blanket 実装

`Any`、`Borrow<T>`、`BorrowMut<T>`、`CloneToUninit`、`From<T>`、`Into<U>`、`ToOwned`、`TryFrom<U>`、`TryInto<U>` を含む標準的な変換・ユーティリティトレイト。

---

本ページは [`proc_macro::Spacing` (stable)](https://doc.rust-lang.org/stable/proc_macro/enum.Spacing.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
