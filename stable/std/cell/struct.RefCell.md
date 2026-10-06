---
title: RefCell
---

# Struct RefCell（要約）

```rust
pub struct RefCell<T: ?Sized> { /* private fields */ }
```

`RefCell` は Rust に内部可変性 (interior mutability) を提供します。コンパイル時の静的チェックの代わりに実行時の借用チェックを使うことで、不変参照の背後にあるデータへの可変アクセスを可能にします。

## 特徴

**目的**: `RefCell` は、借用規則の検証をコンパイラの静的チェックに頼る代わりに実行時へ遅延させることで、不変なコンテナの内側のデータを変更できるようにします。

**借用チェック**:
- `borrow()` — 不変な `Ref<T>` を返す。複数の不変借用を同時に行える
- `borrow_mut()` — 可変な `RefMut<T>` を返す。可変借用は一度に1つだけ
- どちらのメソッドも、実行時に借用規則に違反すると**パニックする**（たとえば不変借用が存在する間に `borrow_mut()` を呼ぶなど）
- パニックしない代替: `try_borrow()` と `try_borrow_mut()` は `Result` を返す

**例**:
```rust
use std::cell::RefCell;

let c = RefCell::new(5);
let borrowed = c.borrow();        // 不変借用
let borrowed2 = c.borrow();       // 複数の不変借用は OK
// let m = c.borrow_mut();        // パニックする - まだ不変に借用されている

// 借用が drop された後なら、可変借用できる
let mut_ref = c.borrow_mut();
*mut_ref = 10;
```

## Cell との関係

**Cell** と **RefCell** の違い:
- **Cell**: `Copy` 型に対して `set()` と `get()` のみを提供する。実行時オーバーヘッドはないが、できることは非常に限られる
- **RefCell**: 任意の型に対して完全な借用のセマンティクス（`borrow()`、`borrow_mut()`）を提供する。借用の状態を実行時に追跡する

## Rc との典型的な組み合わせ

`RefCell` は、単一スレッドの文脈で共有された可変所有権を実現するために、`Rc`（参照カウント）とよく組み合わされます。

```rust
use std::rc::Rc;
use std::cell::RefCell;

let data = Rc::new(RefCell::new(5));
let data_clone = Rc::clone(&data);

*data.borrow_mut() = 10;  // 共有された参照を通じて変更する
```

**注意**: `RefCell` は **`Sync` ではない**ため、スレッドをまたいで使うことはできません。マルチスレッドの場面では `Mutex` や `Arc<Mutex<T>>` を使ってください。

---

本ページは [`std::cell::RefCell` (stable)](https://doc.rust-lang.org/stable/std/cell/struct.RefCell.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/cell/struct.RefCell.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
