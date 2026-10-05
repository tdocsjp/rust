---
title: Instant
---

# Struct Instant（要約）

```rust
pub struct Instant(/* private fields */);
```

`Instant` は**単調増加するクロック**による計測値で、経過時間の測定や時点の比較に使われます。`SystemTime` とは異なり、決して逆行しないことが保証されており、ベンチマークや処理時間の計測、相対的な時間計測に向いています。

## 特徴

- **単調増加** — （稀なハードウェア・OS のバグを除き）常に前へ進む
- **不透明** — エポックからの秒数のような絶対値へ変換できない
- **比較専用** — 他の `Instant` との比較・差分計算にしか使えない
- **非一定** — 個々のクロックの刻みの長さは一定でない場合があり、クロックが時間の遅れを経験することもある

## 典型的な使い方

2つの時点の間の経過時間を測る:

```rust
use std::time::{Duration, Instant};
use std::thread::sleep;

let now = Instant::now();
sleep(Duration::new(2, 0));
println!("{}", now.elapsed().as_secs()); // 2 と表示される
```

## 主なメソッド

- `Instant::now()` — 現在の時点を取得する
- `elapsed()` — この `Instant` が作られてからの経過時間を返す
- `duration_since(earlier)` — 2つの `Instant` 間の経過時間を返す（順序が逆であればゼロに飽和する）
- `checked_duration_since(earlier)` — `Option` を返す。単調性の違反を検出するのに便利

## SystemTime との違い

- `SystemTime`: 絶対的な壁時計時間。システム時刻が調整されると逆行することがある
- `Instant`: 相対的な単調時間。決して逆行しない。間隔の計測に向いている

## 安全性に関する注意

`add()` のような算術操作は、結果が表現可能な範囲を超えるとパニックすることがあります。失敗しうる版として `checked_add()` や `checked_sub()` を使ってください。

---

本ページは [`std::time::Instant` (stable)](https://doc.rust-lang.org/stable/std/time/struct.Instant.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/time/struct.Instant.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
