---
title: Delimiter
---

# Enum Delimiter

一連のトークンツリーがどのように区切られているかを表します。

## 定義

```rust
pub enum Delimiter {
    Parenthesis,
    Brace,
    Bracket,
    None,
}
```

## バリアント

### Parenthesis

```
( ... )
```

**利用可能バージョン:** 1.29.0 から

### Brace

```
{ ... }
```

**利用可能バージョン:** 1.29.0 から

### Bracket

```
[ ... ]
```

**利用可能バージョン:** 1.29.0 から

### None

```
∅ ... ∅
```

**利用可能バージョン:** 1.29.0 から

「マクロ変数」`$var` から来るトークンの周りに現れうる、見えない区切り記号です。`$var` が `1 + 2` であるときの `$var * 3` のようなケースで演算子の優先順位を保持するために重要です。

**注意:** 見えない区切り記号は、トークンストリームが文字列を経由したラウンドトリップを経ると、そのまま残らないことがあります。現在 rustc は、proc マクロの出力における `None` で区切られたトークンのグルーピングを無視することがあります。proc マクロの入力において `macro_rules` マクロによって作成された `None` 区切りのグループだけが、特定の状況下で保持されます。proc マクロによって作成された `None` 区切りのグループは、演算子の優先順位を保持しません。代わりに他の `Delimiter` バリアントを使ってください。これは rustc のバグです（[rust-lang/rust#67062](https://github.com/rust-lang/rust/issues/67062) を参照）。

## トレイト実装

- **Clone** (1.29.0)
- **Copy** (1.29.0)
- **Debug** (1.29.0)
- **Eq** (1.29.0)
- **PartialEq** (1.29.0)
- **StructuralPartialEq** (1.29.0)

## Auto Trait の実装

- Freeze
- RefUnwindSafe
- Send
- Sync
- Unpin
- UnsafeUnpin
- UnwindSafe

## Blanket 実装

Any、Borrow、BorrowMut、CloneToUninit、From、Into、ToOwned、TryFrom、TryInto を含む標準の Rust トレイト実装。

---

本ページは [`proc_macro::Delimiter` (stable)](https://doc.rust-lang.org/stable/proc_macro/enum.Delimiter.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
