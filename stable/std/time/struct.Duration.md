---
title: Duration
---

# Struct Duration（要約）

```rust
pub struct Duration { /* private fields */ }
```

`Duration` は時間の長さを表す構造体で、タイムアウトや時間計測によく使われます。内部的には秒（u64）とナノ秒未満の端数（u32、0〜999,999,999）で保持されます。`Copy` であり、ハッシュ化・比較が可能な、軽量で扱いやすい型です。

## 構築方法

```rust
use std::time::Duration;

Duration::new(5, 500_000_000)           // 5.5秒
Duration::from_secs(5)                  // 5秒
Duration::from_millis(2_569)            // 2.569秒
Duration::from_micros(1_000_002)        // 1.000002秒
Duration::from_nanos(1_000_000_123)     // 1.000000123秒

Duration::from_hours(6)                 // 6時間
Duration::from_mins(10)                 // 10分

Duration::from_secs_f64(2.7)            // 浮動小数点の秒数から
Duration::try_from_secs_f64(2.7)        // Result を返す版
```

## 値の取り出し

```rust
let d = Duration::new(5, 730_023_852);

d.as_secs()           // 5（整数秒のみ）
d.subsec_millis()     // 730（端数部分、ミリ秒）

d.as_millis()         // 5_730（合計ミリ秒）
d.as_nanos()          // 5_730_023_852（合計ナノ秒、u128）

d.as_secs_f64()       // 5.730023852（浮動小数点）
```

## 算術演算

```rust
// 基本の算術（オーバーフロー時はパニック）
d1 + d2
d1 * 3_u32

// checked 系（Option を返す）
d1.checked_add(d2)

// saturating 系（Duration::MAX / Duration::ZERO にクランプする）
d1.saturating_sub(d2)

// 浮動小数点演算
d1.mul_f64(3.14)

// 絶対差（常に正の Duration を返す）
d1.abs_diff(d2)
```

## 範囲と精度

- **精度**: ナノ秒単位（10⁻⁹秒）
- **範囲**: `Duration::ZERO` から `Duration::MAX`（最大で約5,840億年）

## 注意点

- **`Display` を実装していません**: `Duration` は意図的に `Display` を省いています。`{:?}` による `Debug` 表示か、独自の整形を使ってください。
- **負の時間はありません**: `Duration` は常に非負で、アンダーフローする減算は `None` または `Duration::ZERO` を返します。
- **トレイト実装**: `Clone`、`Copy`、`Eq`、`Ord`、`Hash`、`Default`、`Debug`、および標準的な算術演算子すべて。

---

本ページは [`std::time::Duration` (stable)](https://doc.rust-lang.org/stable/std/time/struct.Duration.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/time/struct.Duration.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
