---
title: VecDeque
---

# Struct VecDeque（要約）

```rust
pub struct VecDeque<T, A: Allocator = Global> { /* private fields */ }
```

`VecDeque` は、可変長のリングバッファとして実装された両端キュー (double-ended queue) です。

## 用途

`Vec` が末尾での操作に最適化されているのに対し、`VecDeque` はコレクションの両端での要素の挿入・削除を効率的に行えます。

## 特徴

- **リングバッファでの実装** — 要素は内部で循環するため、先頭が必ずしもインデックス0にあるわけではない
- **両端対応** — 先頭・末尾のどちらからでも _O_(1) で push/pop できる
- **可変長** — 必要に応じて動的にメモリを確保する
- **必ずしも連続していない** — 要素が内部で2つのスライスに分かれていることがある（単一のスライスにするには `make_contiguous()` を使う）

## 主な用途

1. **キュー** — FIFO の動作には `push_back()` と `pop_front()` を使う
2. **デック** — 両端を使う柔軟なアクセスパターン
3. **ソートが必要な操作** — 効率的にソートするために `make_contiguous()` を呼ぶ

## 主なメソッド

```rust
// 構築
let mut deque = VecDeque::new();
let mut deque = VecDeque::with_capacity(10);

// 要素の追加
deque.push_back(value);    // 末尾に追加
deque.push_front(value);   // 先頭に追加

// 要素の削除
deque.pop_back();          // 末尾から削除
deque.pop_front();         // 先頭から削除

// アクセス
deque.front();             // 先頭要素への参照
deque.back();              // 末尾要素への参照
deque[index];              // インデックスアクセス
```

## Vec との比較

- `Vec`: 末尾での操作は速いが、先頭での操作は遅い
- `VecDeque`: 両端での操作が速く、キュー・デックのセマンティクスが必要な場合に適している
- どちらもインデックスによるランダムアクセスに対応
- `VecDeque` はリングバッファの管理のため、わずかに大きいオーバーヘッドを持つことがある

---

本ページは [`std::collections::VecDeque` (stable)](https://doc.rust-lang.org/stable/std/collections/struct.VecDeque.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/collections/struct.VecDeque.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
