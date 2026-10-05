---
title: Iterator
---

# Trait Iterator

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // 多数の提供メソッド（デフォルト実装あり）
}
```

`Iterator` は、要素の並びを扱うための Rust の基本的な抽象です。メソッドチェーンによる遅延評価・合成可能な反復を可能にし、Rust のイテレータ周りのエコシステム全体の中心にあります。

## 必須の要素

- **関連型 `Item`** — 反復対象となる要素の型。
- **必須メソッド `next(&mut self) -> Option<Self::Item>`** — イテレータを1つ進め、次の値を返す。尽きたら `None` を返す。実装しなければならないのはこのメソッドだけで、他のすべてのメソッドはこれをもとに構築されている。

## 提供メソッドの主なカテゴリ

`Iterator` には76以上の提供メソッドがあり、おおよそ次のように分類できます。

- **コンシューマ**（最終的な値を生成して終端する操作）: `count`・`last`・`sum`・`product`（単一の値への縮約）、`collect`（コレクションへ集約）、`fold`・`reduce`（カスタムな縮約）、`any`・`all`（真偽値での判定）、`find`・`position`（検索）
- **アダプタ**（新しいイテレータを生成する変換）: `map`・`filter`・`filter_map`（要素の変換）、`flat_map`・`flatten`（入れ子構造の展開）、`take`・`skip`（部分の切り出し）、`zip`・`chain`（複数イテレータの結合）、`enumerate`（インデックスの付加）、`cycle`（無限の繰り返し）
- **インスペクタ**（変更を伴わない観察）: `peekable` による `peek`（先読み）、`inspect`（ロギング・デバッグ用）
- **比較・順序付け**: `cmp`・`eq`・`lt` など（イテレータ同士の比較）、`max`・`min`・`max_by`・`min_by`（最大・最小の探索）、`is_sorted`（ソート済みかの検証）
- **その他のユーティリティ**: `size_hint`（最適化のためのヒント）、`nth`（インデックスによるランダムアクセス）、`partition`（述語による分割）

この設計により、遅延評価とメソッドの合成を通じて、強力で表現力の高い、効率的なデータ処理が可能になっています。

---

各メソッドの個別の説明文と例は今後追加予定です。最新の個別ページは [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/iter/trait.Iterator.html) を参照してください。

---

本ページは [`std::iter::Iterator` (stable)](https://doc.rust-lang.org/stable/std/iter/trait.Iterator.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
