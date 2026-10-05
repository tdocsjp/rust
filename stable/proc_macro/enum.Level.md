---
title: Level
---

# Enum Level

診断の深刻度レベルを表す、非網羅的な enum です。`proc_macro_diagnostic`（トラッキングイシュー [#54140](https://github.com/rust-lang/rust/issues/54140)）フィーチャーゲートの下にある、nightly 限定の実験的な diagnostic API の一部です。

## 定義

```rust
#[non_exhaustive]
pub enum Level {
    Error,
    Warning,
    Note,
    Help,
}
```

## バリアント

- **Error** - エラーの診断レベル
- **Warning** - 警告の診断レベル
- **Note** - 注記・情報提供の診断レベル
- **Help** - ヘルプメッセージの診断レベル

すべてのバリアントは nightly 限定の実験的機能としてマークされています。

## トレイト実装

### 明示的な実装

- **Clone** - `clone()` を通じて値の複製を返す
- **Copy** - ビット単位のコピーのセマンティクスを実装する
- **Debug** - `Formatter` を使って値を整形する

### Auto Trait の実装

- Freeze
- RefUnwindSafe
- Send
- Sync
- Unpin
- UnsafeUnpin
- UnwindSafe

### Blanket 実装

- Any
- Borrow\<T\>
- BorrowMut\<T\>
- CloneToUninit
- From\<T\>
- Into\<U\>
- ToOwned
- TryFrom\<U\>
- TryInto\<U\>

## 重要な注意

- この enum には `#[non_exhaustive]` が付いており、将来のバージョンで新しいバリアントが追加される可能性があります。パターンマッチングの際は、将来追加される可能性のあるバリアントを扱うためにワイルドカードアーム (`_`) を含めなければなりません。
- これは unstable な nightly 限定の API であり、使用するには `proc_macro_diagnostic` フィーチャーが必要です。

---

本ページは [`proc_macro::Level` (stable)](https://doc.rust-lang.org/stable/proc_macro/enum.Level.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
