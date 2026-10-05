---
title: thread
---

# Module thread（要約）

`thread` モジュールは、OS ネイティブなスレッド機能を提供します。Rust のプログラムは、自身のスタックとローカル状態を持つ並行スレッドを生成でき、スレッド同士はチャネルや `Arc` のような共有メモリのデータ構造を通じて通信します。

## スレッドの生成

クロージャとともに [`thread::spawn`](#functions) を使います。

```rust
use std::thread;

thread::spawn(move || {
    // ここで作業する
});
```

これは「デタッチされた」スレッドを作成し、完了を追跡する方法はありません。

## JoinHandle と join()

スレッドの完了を待つには、返された `JoinHandle` を受け取ります。

```rust
let handle = thread::spawn(move || {
    // ここで作業する
});

let result = handle.join(); // スレッドが終わるまでブロックする
```

`join()` メソッドは `thread::Result<T>` を返します。これはスレッドの最終的な値を持つ `Ok`、またはスレッドがパニックした場合の `Err` のいずれかです。

## スレッドのパニック

スレッドのパニックはスタックの巻き戻しとリソースの解放を引き起こします。パニックは [`catch_unwind`](/stable/std/panic/) で捕捉するか、別のスレッドから `join()` で検出できます。メインスレッドが捕捉されずにパニックすると、プログラム全体が非ゼロの終了コードで終了します。

## スレッドローカルストレージ

[`thread_local!`](/stable/std/) マクロは、各スレッドが独自のコピーを持つスレッドローカル変数を作成します。アクセスは設計上スレッド安全であり、データはスレッドの終了時に破棄されます。値は `'static` でなければならず、内部可変性のために `Cell` や `RefCell` を使うのが一般的です。

## 設定

生成前にスレッドを設定するには [`Builder`](struct.Builder.html) を使います。

```rust
thread::Builder::new()
    .name("thread1".to_string())
    .stack_size(some_size)
    .spawn(move || { /* ... */ });
```

---

本ページは [`std::thread` (stable)](https://doc.rust-lang.org/stable/std/thread/index.html) の要約の非公式日本語訳です。個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/thread/index.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
