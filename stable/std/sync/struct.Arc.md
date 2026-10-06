---
title: Arc
---

# Struct Arc（要約）

```rust
pub struct Arc<T: ?Sized, A: Allocator = Global> { /* private fields */ }
```

`Arc<T>`（Atomically Reference Counted）は、複数スレッドにわたってヒープ確保データの**共有所有権**を可能にする、スレッド安全なスマートポインタです。最後の参照が drop されたときに、内部の値を自動的に解放します。

## Rc との違い

| 特徴 | Arc\<T\> | Rc\<T\> |
|---------|--------|-------|
| スレッド安全 | はい（アトミック操作） | いいえ |
| 性能 | やや遅い（アトミック操作のオーバーヘッド） | 速い |
| 用途 | マルチスレッドのコード | 単一スレッドのコード |

目安としては、基本的には `Rc<T>` を使い（コンパイラがスレッドをまたぐ `Rc` の受け渡しを拒否してくれます）、明示的にマルチスレッドで共有する必要があるときに `Arc<T>` を選びます。

## クローンのセマンティクス

`Arc` をクローンすると参照カウントは増えますが、内部データはクローンされません。

```rust
let arc1 = Arc::new(vec![1, 2, 3]);
let arc2 = Arc::clone(&arc1); // 参照カウントを増やし、確保を共有する
```

## 内部可変性

`Arc<T>` は（他の共有参照と同様に）デフォルトでは可変参照を許しません。変更が必要な場合は、`Mutex` や `RwLock` のような同期プリミティブと組み合わせます。

```rust
use std::sync::{Arc, Mutex};

let counter = Arc::new(Mutex::new(0));
let counter_clone = Arc::clone(&counter);

std::thread::spawn(move || {
    let mut num = counter_clone.lock().unwrap();
    *num += 1;
});
```

変更の頻度が低い場合は、`Arc::make_mut()` による効率的な CoW (clone-on-write) セマンティクスも使えます（他に参照が存在するときだけクローンする）。

## Weak 参照

`Weak<T>` を使って参照の循環を断ち切り、メモリリークを防ぐことができます。`weak.upgrade()` は `Option<Arc<T>>` を返します。`Weak` ポインタは値を生かし続けず、確保だけを生かし続けます。親子関係で、子が親への弱い参照を持つのに適しています。

## 性能上の注意

アトミック操作は通常のメモリアクセスより**コストが高い**です。`Arc<T>` は、スレッドをまたいで共有する必要がある場合や、所有権をきれいにムーブできない場合にのみ使ってください。単一スレッドの場面では、`Rc<T>` の方がアトミック操作のオーバーヘッドなしで良い性能を発揮します。

## Send + Sync

`Arc<T>` は、`T` がそれを実装している場合にのみ `Send` と `Sync` を実装します。つまり `Arc<RefCell<T>>` は、`Arc` 自体がアトミックであっても、`RefCell` がスレッド安全でないため `Send` になりません。

---

本ページは [`std::sync::Arc` (stable)](https://doc.rust-lang.org/stable/std/sync/struct.Arc.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/sync/struct.Arc.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
