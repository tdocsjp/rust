---
title: BTreeMap
---

# Struct BTreeMap（要約）

```rust
pub struct BTreeMap<K, V, A: Allocator = Global> { /* private fields */ }
```

`BTreeMap` は B-Tree に基づく順序付きマップです。`HashMap` が順序を保証しないのに対し、`BTreeMap` はキーでソートされた状態を保ちます。

## 特徴

**順序の保証:**
- キーは（全順序である）`Ord` トレイトを実装している必要がある
- すべての反復（`iter()`・`keys()`・`values()`）は、キーの昇順で要素を返す
- 昇順のキー順を自動的に維持する

**HashMap より BTreeMap を選ぶべき場面:**
- キーへの、ソート済み・順序付きのアクセスが必要なとき
- 範囲検索 (range query) が必要なとき
- 最初・最後の要素の操作が必要なとき
- B-Tree のノード構造により、小さなデータセットではキャッシュ効率が良い
- トレードオフ: `HashMap` よりルックアップが遅い（対数時間 vs 定数時間）

**計算量:**
- 取得・挿入・削除: _O_(log n)
- 反復: _O_(n)（1要素あたり償却定数時間）
- 範囲検索: _O_(log n + k)（k は結果の件数）

## 主なメソッド

**範囲操作:**
```rust
map.range(start..end)        // 部分集合を反復する
map.first_key_value()        // 最小のキーと値の組
map.last_key_value()         // 最大のキーと値の組
map.pop_first() / pop_last()
```

**Entry API:**
```rust
map.entry(key).or_insert(default)
map.entry(key).and_modify(|v| *v += 1)
```

**一括操作:**
- `append()` — 2つのマップをマージする
- `split_off()` — 指定したキーで分割する
- `extract_if()` — 範囲に対して条件付きで削除する

## B-Tree を採用している理由

B-Tree は、現代のアーキテクチャにおいて、二分探索木よりもキャッシュ効率と比較のオーバーヘッドのバランスが良いです。理論上は二分探索木の方が比較回数は少なくなります（log₂n に対して B·log(n)）が、B-Tree は1つのノードに複数の要素を格納することでキャッシュ局所性を改善し、実用上はより良い性能を達成します。

---

本ページは [`std::collections::BTreeMap` (stable)](https://doc.rust-lang.org/stable/std/collections/struct.BTreeMap.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/collections/struct.BTreeMap.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
