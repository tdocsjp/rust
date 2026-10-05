---
title: HashSet
---

# Struct HashSet（要約）

```rust
pub struct HashSet<T, S = RandomState, A: Allocator = Global> { /* private fields */ }
```

`HashSet<T>` はハッシュに基づく集合です。内部的には `HashMap<T, ()>` として実装されており、キーだけが使われ値は捨てられます。

## 要素への要件

`HashSet` の要素は次を実装している必要があります。

- **`Eq`** — 等価性の比較のため
- **`Hash`** — ハッシュ化のため

重要な不変条件: **2つの要素が等しければ、そのハッシュも等しくなければならない**。これに違反することは論理エラーです。

```rust
#[derive(Hash, Eq, PartialEq)]
struct Item { name: String }

let mut set = HashSet::new();
set.insert(Item { name: "example".into() });
```

## 主な集合演算

**和集合 (union)** — 両方の集合のすべての要素:
```rust
let a = HashSet::from([1, 2, 3]);
let b = HashSet::from([3, 4, 5]);
let union: HashSet<_> = a.union(&b).collect();  // [1, 2, 3, 4, 5]
```

**積集合 (intersection)** — 両方の集合に含まれる要素:
```rust
let intersection: HashSet<_> = a.intersection(&b).collect();  // [3]
```

**差集合 (difference)** — 1番目にあって2番目にない要素:
```rust
let diff: HashSet<_> = a.difference(&b).collect();  // [1, 2]
```

**対称差 (symmetric difference)** — どちらか一方にのみ含まれる要素:
```rust
let sym_diff: HashSet<_> = a.symmetric_difference(&b).collect();  // [1, 2, 4, 5]
```

## よく使うメソッド

- `insert(value)` — 要素を追加する。新規なら `true` を返す
- `contains(&value)` — 含まれているか確認する
- `remove(&value)` — 要素を削除する
- `len()`、`is_empty()` — サイズの問い合わせ
- `iter()` — すべての要素を反復する
- `retain(predicate)` — 条件に合う要素だけを残す

## 演算子のサポート

便利さのため、集合はビット演算子にも対応しています。

- `&` — 積集合
- `|` — 和集合
- `^` — 対称差
- `-` — 差集合

```rust
let result = &set1 & &set2;  // 積集合
```

---

本ページは [`std::collections::HashSet` (stable)](https://doc.rust-lang.org/stable/std/collections/struct.HashSet.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/collections/struct.HashSet.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
