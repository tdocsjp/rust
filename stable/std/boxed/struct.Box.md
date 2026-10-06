---
title: Box
---

# Struct Box（要約）

```rust
pub struct Box<T, A: Allocator = Global>(/* private fields */);
```

`Box<T>` は、ヒープ上のデータへの単独所有権 (unique ownership) を提供する、Rust のヒープ確保ポインタ型です。主な用途は次の通りです。

- **トレイトオブジェクト** — 同じトレイトを実装する異なる具体型をまとめて保持する（`Box<dyn Trait>`）。
- **再帰的な型** — 再帰的なデータ構造を可能にする（`Box` を挟むことで無限の型の再帰を断ち切る）。たとえば連結リストの `enum List<T> { Cons(T, Box<List<T>>), Nil }`。
- **大きなデータ** — スタック上のコピーを避け、大きな値をヒープへムーブする。
- **サイズ不定の型** — スライスやトレイトオブジェクトのような、サイズがコンパイル時に決まらない型を扱う。
- **async のための Pin** — `Box::pin()` で固定 (pin) されたヒープ確保を作る。

## メモリレイアウトと確保

- カスタマイズ可能なアロケータ（デフォルトは `Global`）を使って確保する。
- ゼロサイズ型は実際には確保を行わない。
- アロケータに対して汎用的（`Box<T, A: Allocator>`）。`allocator_api` フィーチャーでカスタムアロケータに対応。

## 主なメソッド

- 構築: `Box::new(x)`（基本のヒープ確保）、`Box::new_in(x, alloc)`（カスタムアロケータでの確保）、`Box::new_uninit()` / `Box::new_zeroed()`（初期化を後回しにする）
- 変換: `Box::into_raw(b)` / `Box::from_raw(ptr)`、`Box::into_non_null(b)` / `Box::from_non_null(ptr)`、`Box::downcast::<T>()`（トレイトオブジェクトの型変換、`Result` を返す）
- 関数的操作: `Box::map(b, f)` / `Box::try_map(b, f)`（確保を再利用しつつ中身を変換する）

## 主な保証

- 所有権の移動 — `Box` が drop されるとデストラクタが自動的にメモリを解放する。
- `Deref`/`DerefMut` を実装しており、中身へ透過的にアクセスできる。
- `T` の実装に応じて `Send`/`Sync` を実装する。
- `as_ptr()`、`as_mut_ptr()`、`as_non_null()` は参照を生成しないため、低レベルコードで生ポインタと安全に混用できる。
- `downcast()` は安全。`downcast_unchecked()` は nightly 限定。

最も一般的な使い方は、異種混合のコレクションのために `Box<dyn Trait>` を使うパターンと、再帰的な型やスタックからヒープへの大きなムーブのために `Box<T>` を使うパターンです。

---

本ページは [`std::boxed::Box` (stable)](https://doc.rust-lang.org/stable/std/boxed/struct.Box.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/boxed/struct.Box.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
