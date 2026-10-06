---
title: Mutex
---

# Struct Mutex（要約）

```rust
pub struct Mutex<T: ?Sized> { /* private fields */ }
```

`Mutex<T>` は、ロックが使えるようになるまでスレッドをブロックすることで、共有データへの並行アクセスから保護する、排他制御のプリミティブです。drop 時に自動でアンロックする RAII ガードを使います。

## 主なメソッド

### `lock()` — ブロッキングでの取得

- 動作: ミューテックスを獲得できるまで現在のスレッドをブロックする
- 戻り値: `LockResult<MutexGuard<'_, T>>`（RAII ガードを包む Result）
- パニック: 現在のスレッドが既にロックを保持している場合、パニックする可能性がある
- ポイズニング: ロックを保持していた別スレッドがパニックした場合、`Err(PoisonError)` を返す

### `try_lock()` — 非ブロッキングでの取得

- 動作: ブロックせずにロックの取得を試みる
- 戻り値: `TryLockResult<MutexGuard<'_, T>>`
- エラー: すでにロックされていれば `TryLockError::WouldBlock`、ポイズンされていれば `TryLockError::Poisoned` を返す
- 利点: 決してブロックしないため、非ブロッキングなコードパスに使える

## ポイズニング戦略

ミューテックスを保持しているスレッドがパニックすると、そのミューテックスは**ポイズン状態**になります。以降のロック取得は `Err(PoisonError)` を返します。これはあくまで助言的なものです。ポイズンされていても `poisoned.into_inner()` でガードを取り出して復旧できますし、`clear_poison()` で状態をリセットすることもできます。

```rust
let guard = match mutex.lock() {
    Ok(g) => g,
    Err(poisoned) => poisoned.into_inner(), // 復旧する
};
```

## RAII によるアンロック

`MutexGuard` はスコープを抜けると `Drop` によって自動的にアンロックされ、ロックが必ず解放されることを保証します。

```rust
{
    let mut data = mutex.lock().unwrap();
    *data += 1;
} // ここでガードが drop され、ロックが解放される
```

## Arc との組み合わせ

スレッド安全な共有所有権のためには、`Mutex<T>` を `Arc` と組み合わせます。

```rust
let data = Arc::new(Mutex::new(0));
for _ in 0..N {
    let data = Arc::clone(&data);
    thread::spawn(move || {
        *data.lock().unwrap() += 1;
    });
}
```

`Arc` が複数スレッドでの所有権の共有を可能にし、`Mutex` が内部データへの安全な並行アクセスを保証します。

---

本ページは [`std::sync::Mutex` (stable)](https://doc.rust-lang.org/stable/std/sync/struct.Mutex.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/sync/struct.Mutex.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
