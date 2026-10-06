---
title: Rc
---

# Struct Rc（要約）

```rust
pub struct Rc<T: ?Sized, A: Allocator = Global> { /* private fields */ }
```

`Rc<T>`（Reference Counted）は、**単一スレッド内での共有所有権**を可能にする型です。複数の所有者が同じヒープ上の確保を共有でき、最後の参照が drop されたときに自動的に解放されます。

## 特徴

**単一スレッド専用**: `Rc` は `!Send`・`!Sync` であり、並行なコードには向きません。マルチスレッドが必要な場合は `Arc` を使ってください。

**浅いクローン**: `Rc::clone(&x)` は同じ確保への新しいポインタを作り、参照カウントを増やすだけです。安価であり、データを深くコピーすることはありません。

```rust
let five = Rc::new(5);
let also_five = Rc::clone(&five); // 安価。参照カウントを増やすだけ
```

## 内部可変性との組み合わせ

`Rc` はデフォルトで不変です。共有された可変性が必要な場合は、`Cell` や `RefCell` と組み合わせます。

- `Rc<Cell<T>>` — コンパイル時の借用チェック
- `Rc<RefCell<T>>` — `borrow_mut()` による実行時の借用チェック

```rust
use std::rc::Rc;
use std::cell::RefCell;

let value = Rc::new(RefCell::new(5));
*value.borrow_mut() += 1;
```

## Weak 参照

`Rc::downgrade()` は、解放を妨げない `Weak<T>` ポインタを作成します。循環参照を断ち切るのに有用です。`weak.upgrade()` は `Option<Rc<T>>` を返します。親子関係では、子から親への参照に `Weak` を使うことで、循環によるメモリリークを避けられます。

## Rc を使うべきでない場面

- マルチスレッドのコード — 代わりに `Arc<T>` を使う
- スレッド安全なトレイトオブジェクトが必要な場合 — `Arc<dyn Trait>` を使う
- パフォーマンスが重要なホットパス — オーバーヘッドがあるため、所有値やライフタイム借用を検討する
- `Send + Sync` が必要な場合 — `Rc` は明示的にこれらを実装していない

## よく使う操作

| 操作 | 動作 |
|-----------|----------|
| `Rc::new(value)` | 新しい Rc を作る |
| `Rc::clone(&x)` | 参照カウントを増やす |
| `Rc::strong_count(&x)` | 強い参照の数を取得する |
| `Rc::weak_count(&x)` | 弱い参照の数を取得する |
| `Rc::try_unwrap(x)` | 唯一の所有者であれば値を取り出す |
| `Rc::make_mut(&mut x)` | 排他アクセスを確保する（必要ならクローンする） |

---

本ページは [`std::rc::Rc` (stable)](https://doc.rust-lang.org/stable/std/rc/struct.Rc.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/rc/struct.Rc.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
