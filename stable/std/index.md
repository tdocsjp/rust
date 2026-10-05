---
title: std
---

# Rust 標準ライブラリ ドキュメント

## Rust 標準ライブラリ

Rust 標準ライブラリは、ポータブルな Rust ソフトウェアの基盤であり、Rust エコシステム全体のための、最小限かつ実戦で鍛えられた共有の抽象の集まりです。[`Vec<T>`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html) や [`Option<T>`](https://doc.rust-lang.org/stable/std/option/enum.Option.html) といったコア型、言語プリミティブに対するライブラリ定義の操作、標準マクロ、[I/O](https://doc.rust-lang.org/stable/std/io/index.html) や[マルチスレッド](https://doc.rust-lang.org/stable/std/thread/index.html)など、多くのものを提供します。

`std` はデフォルトですべての Rust クレートから利用できます。そのため、標準ライブラリには [`use`](https://doc.rust-lang.org/stable/book/ch07-02-defining-modules-to-control-scope-and-privacy.html) 文の中で `std` というパスを通じてアクセスでき、[`use std::env`](https://doc.rust-lang.org/stable/std/env/index.html) のように書けます。

## このドキュメントの読み方

探しているものの名前がすでに分かっているなら、ページ上部の検索ボタンを使うのが一番速い方法です。

そうでなければ、次の便利なセクションへジャンプするとよいでしょう。

- [`std::*` モジュール](https://doc.rust-lang.org/stable/std/index.html#modules)
- [プリミティブ型](https://doc.rust-lang.org/stable/std/index.html#primitives)
- [標準マクロ](https://doc.rust-lang.org/stable/std/index.html#macros)
- [Rust プレリュード](https://doc.rust-lang.org/stable/std/prelude/index.html)

これが初めてなら、標準ライブラリのドキュメントは気軽にざっと眺められるように書かれています。気になるものをクリックしていけば、だいたい面白い場所に行き着くはずです。とはいえ、見逃したくない重要な部分もあるので、標準ライブラリとそのドキュメントを一通り案内する以下を読み進めてください。

標準ライブラリの内容に慣れてきたら、文章の冗長さが気になってくるかもしれません。その段階になったら、ページ上部付近の「Summary」ボタンを押して、もっとざっと見渡せる表示に折りたたむとよいでしょう。

ページの上部を見ているあいだに、「Source」リンクにも注目してください。Rust の API ドキュメントにはソースコードが添えられており、読むことが推奨されています。標準ライブラリのソースは一般に質が高く、裏側を覗いてみると発見があることも多いです。

## 標準ライブラリのドキュメントには何が書かれているか

まず、Rust 標準ライブラリは、このページのさらに下にすべて列挙されている、いくつかの目的別モジュールに分かれています。これらのモジュールは Rust のすべてを鍛え上げる土台であり、[`std::slice`](https://doc.rust-lang.org/stable/std/slice/index.html) や [`std::cmp`](https://doc.rust-lang.org/stable/std/cmp/index.html) といった堂々とした名前を持っています。各モジュールのドキュメントには通常、そのモジュールの概要と例が含まれており、ライブラリに親しむための賢い出発点になります。

次に、プリミティブ型に対する暗黙のメソッドもここに文書化されています。これは2つの理由で混乱の元になりがちです。

1. プリミティブはコンパイラによって実装されていますが、標準ライブラリはプリミティブ型に対してメソッドを直接実装しており（これを行うのはこのライブラリだけです）、それらはプリミティブについてのセクションに文書化されています。
2. 標準ライブラリは、*プリミティブ型と同じ名前の*モジュールを多数エクスポートしています。これらはそのプリミティブ型に関連する追加の項目を定義していますが、肝心のメソッドは定義していません。

たとえば、[プリミティブ型 `char` のページ](https://doc.rust-lang.org/stable/std/primitive.char.html)には文字に対して呼び出せるすべてのメソッドが列挙されており（非常に有用です）、一方 [モジュール `std::char` のページ](https://doc.rust-lang.org/stable/std/char/index.html)には、それらのメソッドが生成するイテレータ型やエラー型が文書化されています（あまり使うことはありません）。

プリミティブである [`str`](https://doc.rust-lang.org/stable/std/primitive.str.html) と [`[T]`](https://doc.rust-lang.org/stable/std/primitive.slice.html)（「スライス」とも呼ばれます）のドキュメントにも注目してください。[`String`](https://doc.rust-lang.org/stable/std/string/struct.String.html) や [`Vec<T>`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html) に対する多くのメソッド呼び出しは、実際には[デリファレンス型強制](https://doc.rust-lang.org/stable/book/ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods)を経て、それぞれ [`str`](https://doc.rust-lang.org/stable/std/primitive.str.html) と [`[T]`](https://doc.rust-lang.org/stable/std/primitive.slice.html) のメソッドを呼んでいます。

3つ目に、標準ライブラリは[Rust プレリュード](https://doc.rust-lang.org/stable/std/prelude/index.html)を定義しています。これは、すべてのクレートのすべてのモジュールにインポートされる、主にトレイトからなる小さな項目の集まりです。プレリュードに含まれるトレイトは至るところで使われているため、プレリュードのドキュメントはライブラリを学ぶための良い入口になります。

最後に、標準ライブラリは多数の標準マクロをエクスポートしており、このページに列挙しています（厳密に言えば、すべての標準マクロが標準ライブラリで定義されているわけではなく、一部はコンパイラによって定義されていますが、同様にここで文書化されています）。プレリュードと同様、標準マクロもデフォルトですべてのクレートにインポートされます。

## ドキュメントへの貢献

[こちら](https://rustc-dev-guide.rust-lang.org/contributing.html#writing-documentation)にある Rust のコントリビューションガイドラインをご覧ください。このドキュメントのソースは [GitHub](https://github.com/rust-lang/rust) の `library/std/` ディレクトリにあります。変更を加えるには、まずガイドラインを読み、提案する変更に対してプルリクエストを送ってください。

貢献は歓迎されています！ドキュメントの改善できる部分を見つけたら、PR を送るか、まず [Zulip](https://rust-lang.zulipchat.com/#narrow/channel/219381-t-libs/) の #t-libs で私たちに相談してください。

## Rust 標準ライブラリ ツアー

このクレートドキュメントの残りの部分は、Rust 標準ライブラリの注目すべき機能を紹介することに充てられています。

### コンテナとコレクション

[`option`](https://doc.rust-lang.org/stable/std/option/index.html) モジュールと [`result`](https://doc.rust-lang.org/stable/std/result/index.html) モジュールは、オプショナル型とエラーハンドリング型である [`Option<T>`](https://doc.rust-lang.org/stable/std/option/enum.Option.html) と [`Result<T, E>`](https://doc.rust-lang.org/stable/std/result/enum.Result.html) を定義しています。[`iter`](https://doc.rust-lang.org/stable/std/iter/index.html) モジュールは Rust のイテレータトレイトである [`Iterator`](https://doc.rust-lang.org/stable/std/iter/trait.Iterator.html) を定義しており、これは[`for`](https://doc.rust-lang.org/stable/book/ch03-05-control-flow.html#looping-through-a-collection-with-for)ループと組み合わせてコレクションにアクセスするのに使われます。

標準ライブラリは、連続したメモリ領域を扱うための3つの一般的な方法を提供しています。

- [`Vec<T>`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html) - 実行時にサイズ変更可能な、ヒープ確保の*ベクタ*。
- [`[T; N]`](https://doc.rust-lang.org/stable/std/primitive.array.html) - コンパイル時に固定サイズのインラインな*配列*。
- [`[T]`](https://doc.rust-lang.org/stable/std/primitive.slice.html) - ヒープ確保かどうかを問わず、他の種類の連続したストレージへの、動的サイズの*スライス*。

スライスはある種の*ポインタ*を通じてのみ扱うことができ、そのため次のような様々な種類があります。

- `&[T]` - *共有スライス*
- `&mut [T]` - *可変スライス*
- [`Box<[T]>`](https://doc.rust-lang.org/stable/std/boxed/index.html) - *所有スライス*

[`str`](https://doc.rust-lang.org/stable/std/primitive.str.html)、つまり UTF-8 文字列スライスはプリミティブ型であり、標準ライブラリはこれに対して多数のメソッドを定義しています。Rust の [`str`](https://doc.rust-lang.org/stable/std/primitive.str.html) は通常、不変の参照として、つまり `&str` としてアクセスされます。文字列を構築したり変更したりするには、所有権を持つ [`String`](https://doc.rust-lang.org/stable/std/string/struct.String.html) を使ってください。

文字列への変換には [`format!`](https://doc.rust-lang.org/stable/std/macro.format.html) マクロを、文字列からの変換には [`FromStr`](https://doc.rust-lang.org/stable/std/str/trait.FromStr.html) トレイトを使います。

データは、参照カウント方式のボックスである [`Rc`](https://doc.rust-lang.org/stable/std/rc/struct.Rc.html) 型に置くことで共有できます。さらにそれを [`Cell`](https://doc.rust-lang.org/stable/std/cell/struct.Cell.html) や [`RefCell`](https://doc.rust-lang.org/stable/std/cell/struct.RefCell.html) に入れれば、共有だけでなく変更もできるようになります。同様に、並行処理の文脈では、アトミックに参照カウントされるボックス [`Arc`](https://doc.rust-lang.org/stable/std/sync/struct.Arc.html) と [`Mutex`](https://doc.rust-lang.org/stable/std/sync/struct.Mutex.html) を組にして同じ効果を得るのが一般的です。

[`collections`](https://doc.rust-lang.org/stable/std/collections/index.html) モジュールは、マップ、セット、連結リストなど、よく使われるコレクション型を定義しており、その中には一般的な [`HashMap<K, V>`](https://doc.rust-lang.org/stable/std/collections/struct.HashMap.html) も含まれます。

### プラットフォームの抽象化と I/O

基本的なデータ型のほかに、標準ライブラリは主に、よく使われるプラットフォーム、とりわけ Windows と Unix 系統との差異を抽象化することに重点を置いています。

[ファイル](https://doc.rust-lang.org/stable/std/fs/struct.File.html)、[TCP](https://doc.rust-lang.org/stable/std/net/struct.TcpStream.html)、[UDP](https://doc.rust-lang.org/stable/std/net/struct.UdpSocket.html) といった一般的な種類の I/O は、[`io`](https://doc.rust-lang.org/stable/std/io/index.html)、[`fs`](https://doc.rust-lang.org/stable/std/fs/index.html)、[`net`](https://doc.rust-lang.org/stable/std/net/index.html) モジュールで定義されています。

[`thread`](https://doc.rust-lang.org/stable/std/thread/index.html) モジュールには Rust のスレッド抽象が含まれています。[`sync`](https://doc.rust-lang.org/stable/std/sync/index.html) には、[`atomic`](https://doc.rust-lang.org/stable/std/sync/atomic/index.html)、[`mpmc`](https://doc.rust-lang.org/stable/std/sync/mpmc/index.html)、[`mpsc`](https://doc.rust-lang.org/stable/std/sync/mpsc/index.html)（メッセージパッシング用のチャネル型を含む）など、さらなる基本的な共有メモリ型が含まれています。

## `main()` の前後での利用

標準ライブラリの多くの部分は `main()` の前後でも動作することが期待されていますが、これはテストによって保証されているわけではありません。サポートしたい各プラットフォームで自分自身のテストを書いて実行することを推奨します。つまり、`std` を main の前後で使うこと、特に OS やグローバル状態と相互作用する機能を使うことは、安定性・ポータビリティの保証の対象外であり、ベストエフォートでのみ提供されます。とはいえ、バグ報告は歓迎します。

一方、`core` と `alloc` はこうした環境でも動作する可能性が高いですが、パニック・OOM 処理・アロケータといったフック可能な挙動については、そのフックの互換性にも依存するという留保があります。

main の外では一部の機能の挙動が変わることもあります。たとえば標準入出力がバッファリングされなくなったり、一部のパニックが abort になったり、バックトレースにシンボルが付かなくなったりする、などです。

既知の制限の一覧（網羅的ではありません）:

- main 後でのスレッドローカルの利用。これは以下のような追加の機能にも影響します:
  - [`thread::current()`](https://doc.rust-lang.org/stable/std/thread/fn.current.html)
- UNIX では、main の前にファイルディスクリプタ 0、1、2 が変更されていない場合があります（main の間は開いていることが保証されており、プログラム開始時に開いていなければ /dev/null へ O_RDWR で開かれます）

---

## プリミティブ型

- **[array](https://doc.rust-lang.org/stable/std/primitive.array.html)** - 要素型 `T` と非負のコンパイル時定数サイズ `N` を持つ、`[T; N]` と表記される固定サイズ配列。
- **[bool](https://doc.rust-lang.org/stable/std/primitive.bool.html)** - 真偽値型。
- **[char](https://doc.rust-lang.org/stable/std/primitive.char.html)** - 文字型。
- **[f32](https://doc.rust-lang.org/stable/std/primitive.f32.html)** - 32 ビット浮動小数点型（具体的には IEEE 754-2008 で定義される "binary32" 型）。
- **[f64](https://doc.rust-lang.org/stable/std/primitive.f64.html)** - 64 ビット浮動小数点型（具体的には IEEE 754-2008 で定義される "binary64" 型）。
- **[fn](https://doc.rust-lang.org/stable/std/primitive.fn.html)** - `fn(usize) -> bool` のような関数ポインタ。
- **[i8](https://doc.rust-lang.org/stable/std/primitive.i8.html)** - 8 ビット符号付き整数型。
- **[i16](https://doc.rust-lang.org/stable/std/primitive.i16.html)** - 16 ビット符号付き整数型。
- **[i32](https://doc.rust-lang.org/stable/std/primitive.i32.html)** - 32 ビット符号付き整数型。
- **[i64](https://doc.rust-lang.org/stable/std/primitive.i64.html)** - 64 ビット符号付き整数型。
- **[i128](https://doc.rust-lang.org/stable/std/primitive.i128.html)** - 128 ビット符号付き整数型。
- **[isize](https://doc.rust-lang.org/stable/std/primitive.isize.html)** - ポインタサイズの符号付き整数型。
- **[pointer](https://doc.rust-lang.org/stable/std/primitive.pointer.html)** - 生の unsafe なポインタ、`*const T` と `*mut T`。
- **[reference](https://doc.rust-lang.org/stable/std/primitive.reference.html)** - 参照、`&T` と `&mut T`。
- **[slice](https://doc.rust-lang.org/stable/std/primitive.slice.html)** - 連続したシーケンスへの動的サイズのビュー、`[T]`。
- **[str](https://doc.rust-lang.org/stable/std/primitive.str.html)** - 文字列スライス。
- **[tuple](https://doc.rust-lang.org/stable/std/primitive.tuple.html)** - 有限の異種混合シーケンス、`(T, U, ..)`。
- **[u8](https://doc.rust-lang.org/stable/std/primitive.u8.html)** - 8 ビット符号なし整数型。
- **[u16](https://doc.rust-lang.org/stable/std/primitive.u16.html)** - 16 ビット符号なし整数型。
- **[u32](https://doc.rust-lang.org/stable/std/primitive.u32.html)** - 32 ビット符号なし整数型。
- **[u64](https://doc.rust-lang.org/stable/std/primitive.u64.html)** - 64 ビット符号なし整数型。
- **[u128](https://doc.rust-lang.org/stable/std/primitive.u128.html)** - 128 ビット符号なし整数型。
- **[unit](https://doc.rust-lang.org/stable/std/primitive.unit.html)** - `()` 型。「ユニット」とも呼ばれます。
- **[usize](https://doc.rust-lang.org/stable/std/primitive.usize.html)** - ポインタサイズの符号なし整数型。
- **[f16](https://doc.rust-lang.org/stable/std/primitive.f16.html)** - 16 ビット浮動小数点型（具体的には IEEE 754-2008 で定義される "binary16" 型）。（実験的）
- **[f128](https://doc.rust-lang.org/stable/std/primitive.f128.html)** - 128 ビット浮動小数点型（具体的には IEEE 754-2008 で定義される "binary128" 型）。（実験的）
- **[never](https://doc.rust-lang.org/stable/std/primitive.never.html)** - `!` 型。「never」とも呼ばれます。（実験的）

---

## モジュール

- **[alloc](https://doc.rust-lang.org/stable/std/alloc/index.html)** - メモリ確保 API。
- **[any](https://doc.rust-lang.org/stable/std/any/index.html)** - 動的型付けや型のリフレクションのためのユーティリティ。
- **[arch](https://doc.rust-lang.org/stable/std/arch/index.html)** - SIMD とベンダー intrinsics のモジュール。
- **[array](https://doc.rust-lang.org/stable/std/array/index.html)** - 配列プリミティブ型のためのユーティリティ。
- **[ascii](https://doc.rust-lang.org/stable/std/ascii/index.html)** - ASCII の文字列・文字に対する操作。
- **[backtrace](https://doc.rust-lang.org/stable/std/backtrace/index.html)** - OS スレッドのスタックバックトレース取得のサポート。
- **[borrow](https://doc.rust-lang.org/stable/std/borrow/index.html)** - 借用データを扱うためのモジュール。
- **[boxed](https://doc.rust-lang.org/stable/std/boxed/index.html)** - ヒープ確保のための `Box<T>` 型。
- **[cell](https://doc.rust-lang.org/stable/std/cell/index.html)** - 共有可能な可変コンテナ。
- **[char](https://doc.rust-lang.org/stable/std/char/index.html)** - `char` プリミティブ型のためのユーティリティ。
- **[clone](https://doc.rust-lang.org/stable/std/clone/index.html)** - 「暗黙にはコピーできない」型のための `Clone` トレイト。
- **[cmp](https://doc.rust-lang.org/stable/std/cmp/index.html)** - 値の比較と順序付けのためのユーティリティ。
- **[collections](https://doc.rust-lang.org/stable/std/collections/index.html)** - コレクション型。
- **[convert](https://doc.rust-lang.org/stable/std/convert/index.html)** - 型間の変換のためのトレイト。
- **[default](https://doc.rust-lang.org/stable/std/default/index.html)** - デフォルト値を持つ型のための `Default` トレイト。
- **[env](https://doc.rust-lang.org/stable/std/env/index.html)** - プロセスの環境の検査と操作。
- **[error](https://doc.rust-lang.org/stable/std/error/index.html)** - エラーを扱うためのインタフェース。
- **[f32](https://doc.rust-lang.org/stable/std/f32/index.html)** - `f32` 単精度浮動小数点型の定数。
- **[f64](https://doc.rust-lang.org/stable/std/f64/index.html)** - `f64` 倍精度浮動小数点型の定数。
- **[ffi](https://doc.rust-lang.org/stable/std/ffi/index.html)** - FFI バインディングに関連するユーティリティ。
- **[fmt](https://doc.rust-lang.org/stable/std/fmt/index.html)** - `String` の整形・出力のためのユーティリティ。
- **[fs](https://doc.rust-lang.org/stable/std/fs/index.html)** - ファイルシステム操作。
- **[future](https://doc.rust-lang.org/stable/std/future/index.html)** - 非同期処理の基本機能。
- **[hash](https://doc.rust-lang.org/stable/std/hash/index.html)** - 汎用的なハッシュのサポート。
- **[hint](https://doc.rust-lang.org/stable/std/hint/index.html)** - コードの出力や最適化の方法に影響するヒントをコンパイラに与える。
- **[i8](https://doc.rust-lang.org/stable/std/i8/index.html)** - [`i8` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.i8.html)に対する冗長な定数モジュール。（非推奨）
- **[i16](https://doc.rust-lang.org/stable/std/i16/index.html)** - [`i16` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.i16.html)に対する冗長な定数モジュール。（非推奨）
- **[i32](https://doc.rust-lang.org/stable/std/i32/index.html)** - [`i32` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.i32.html)に対する冗長な定数モジュール。（非推奨）
- **[i64](https://doc.rust-lang.org/stable/std/i64/index.html)** - [`i64` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.i64.html)に対する冗長な定数モジュール。（非推奨）
- **[i128](https://doc.rust-lang.org/stable/std/i128/index.html)** - [`i128` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.i128.html)に対する冗長な定数モジュール。（非推奨）
- **[io](https://doc.rust-lang.org/stable/std/io/index.html)** - コア I/O 機能のためのトレイト、ヘルパー、型定義。
- **[isize](https://doc.rust-lang.org/stable/std/isize/index.html)** - [`isize` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.isize.html)に対する冗長な定数モジュール。（非推奨）
- **[iter](https://doc.rust-lang.org/stable/std/iter/index.html)** - 合成可能な外部イテレーション。
- **[marker](https://doc.rust-lang.org/stable/std/marker/index.html)** - 型の基本的な性質を表すプリミティブなトレイトと型。
- **[mem](https://doc.rust-lang.org/stable/std/mem/index.html)** - メモリ、値、型を扱うための基本的な関数。
- **[net](https://doc.rust-lang.org/stable/std/net/index.html)** - TCP/UDP 通信のためのネットワークプリミティブ。
- **[num](https://doc.rust-lang.org/stable/std/num/index.html)** - 数値に関する追加機能。
- **[ops](https://doc.rust-lang.org/stable/std/ops/index.html)** - オーバーロード可能な演算子。
- **[option](https://doc.rust-lang.org/stable/std/option/index.html)** - オプショナルな値。
- **[os](https://doc.rust-lang.org/stable/std/os/index.html)** - OS 固有の機能。
- **[panic](https://doc.rust-lang.org/stable/std/panic/index.html)** - 標準ライブラリにおけるパニックのサポート。
- **[path](https://doc.rust-lang.org/stable/std/path/index.html)** - クロスプラットフォームなパス操作。
- **[pin](https://doc.rust-lang.org/stable/std/pin/index.html)** - データをメモリ上の位置に固定する型。
- **[prelude](https://doc.rust-lang.org/stable/std/prelude/index.html)** - Rust プレリュード。
- **[primitive](https://doc.rust-lang.org/stable/std/primitive/index.html)** - このモジュールはプリミティブ型を再エクスポートし、他に宣言された型によって隠されない形で利用できるようにします。
- **[process](https://doc.rust-lang.org/stable/std/process/index.html)** - プロセスを扱うためのモジュール。
- **[ptr](https://doc.rust-lang.org/stable/std/ptr/index.html)** - 生ポインタを通じてメモリを手動で管理する。
- **[range](https://doc.rust-lang.org/stable/std/range/index.html)** - range 型の置き換え。
- **[rc](https://doc.rust-lang.org/stable/std/rc/index.html)** - シングルスレッド向けの参照カウントポインタ。「Rc」は「Reference Counted」の略です。
- **[result](https://doc.rust-lang.org/stable/std/result/index.html)** - `Result` 型によるエラーハンドリング。
- **[slice](https://doc.rust-lang.org/stable/std/slice/index.html)** - スライスプリミティブ型のためのユーティリティ。
- **[str](https://doc.rust-lang.org/stable/std/str/index.html)** - `str` プリミティブ型のためのユーティリティ。
- **[string](https://doc.rust-lang.org/stable/std/string/index.html)** - UTF-8 エンコードされた、サイズ可変な文字列。
- **[sync](https://doc.rust-lang.org/stable/std/sync/index.html)** - 便利な同期プリミティブ。
- **[task](https://doc.rust-lang.org/stable/std/task/index.html)** - 非同期タスクを扱うための型とトレイト。
- **[thread](https://doc.rust-lang.org/stable/std/thread/index.html)** - ネイティブスレッド。
- **[time](https://doc.rust-lang.org/stable/std/time/index.html)** - 時間の定量化。
- **[u8](https://doc.rust-lang.org/stable/std/u8/index.html)** - [`u8` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.u8.html)に対する冗長な定数モジュール。（非推奨）
- **[u16](https://doc.rust-lang.org/stable/std/u16/index.html)** - [`u16` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.u16.html)に対する冗長な定数モジュール。（非推奨）
- **[u32](https://doc.rust-lang.org/stable/std/u32/index.html)** - [`u32` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.u32.html)に対する冗長な定数モジュール。（非推奨）
- **[u64](https://doc.rust-lang.org/stable/std/u64/index.html)** - [`u64` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.u64.html)に対する冗長な定数モジュール。（非推奨）
- **[u128](https://doc.rust-lang.org/stable/std/u128/index.html)** - [`u128` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.u128.html)に対する冗長な定数モジュール。（非推奨）
- **[usize](https://doc.rust-lang.org/stable/std/usize/index.html)** - [`usize` プリミティブ型](https://doc.rust-lang.org/stable/std/primitive.usize.html)に対する冗長な定数モジュール。（非推奨）
- **[vec](https://doc.rust-lang.org/stable/std/vec/index.html)** - ヒープ確保された内容を持つ、連続した可変長の配列型。`Vec<T>` と表記されます。
- **[async_iter](https://doc.rust-lang.org/stable/std/async_iter/index.html)** - 合成可能な非同期イテレーション。（実験的）
- **[autodiff](https://doc.rust-lang.org/stable/std/autodiff/index.html)** - このモジュールは自動微分のサポートを提供します。`autodiff_forward` マクロと `autodiff_reverse` マクロの違いや使い方の詳細については、それぞれのドキュメントを参照してください。（実験的）
- **[bstr](https://doc.rust-lang.org/stable/std/bstr/index.html)** - `ByteStr` 型と `ByteString` 型、およびそのトレイト実装。（実験的）
- **[f16](https://doc.rust-lang.org/stable/std/f16/index.html)** - `f16` 半精度浮動小数点型の定数。（実験的）
- **[f128](https://doc.rust-lang.org/stable/std/f128/index.html)** - `f128` 四倍精度浮動小数点型の定数。（実験的）
- **[field](https://doc.rust-lang.org/stable/std/field/index.html)** - フィールドリフレクション。（実験的）
- **[from](https://doc.rust-lang.org/stable/std/from/index.html)** - unstable な `From` derive マクロを含む unstable モジュール。（実験的）
- **[intrinsics](https://doc.rust-lang.org/stable/std/intrinsics/index.html)** - コンパイラ intrinsics。（実験的）
- **[offload](https://doc.rust-lang.org/stable/std/offload/index.html)** - このモジュールは GPU オフロードのサポートを提供します。`offload_kernel` マクロや `offload!` マクロに関する技術的な詳細は、それぞれのドキュメントを参照してください。（実験的）
- **[pat](https://doc.rust-lang.org/stable/std/pat/index.html)** - `pattern_type` マクロをエクスポートするためのヘルパーモジュール。（実験的）
- **[random](https://doc.rust-lang.org/stable/std/random/index.html)** - ランダムな値の生成。（実験的）
- **[simd](https://doc.rust-lang.org/stable/std/simd/index.html)** - ポータブルな SIMD モジュール。（実験的）
- **[unsafe_binder](https://doc.rust-lang.org/stable/std/unsafe_binder/index.html)** - 型を unsafe binder に変換したり元に戻したりするための演算子。（実験的）
- **[view](https://doc.rust-lang.org/stable/std/view/index.html)** - `view_types` マクロをエクスポートするためのヘルパーモジュール。（実験的）

---

## マクロ

- **[assert](https://doc.rust-lang.org/stable/std/macro.assert.html)** - 実行時にブール式が `true` であることをアサートする。
- **[assert_eq](https://doc.rust-lang.org/stable/std/macro.assert_eq.html)** - ([`PartialEq`](https://doc.rust-lang.org/stable/std/cmp/trait.PartialEq.html) を使って)2つの式が等しいことをアサートする。
- **[assert_matches](https://doc.rust-lang.org/stable/std/macro.assert_matches.html)** - 式が与えられたパターンにマッチすることをアサートする。
- **[assert_ne](https://doc.rust-lang.org/stable/std/macro.assert_ne.html)** - ([`PartialEq`](https://doc.rust-lang.org/stable/std/cmp/trait.PartialEq.html) を使って)2つの式が等しくないことをアサートする。
- **[cfg](https://doc.rust-lang.org/stable/std/macro.cfg.html)** - コンパイル時に設定フラグのブール組み合わせを評価する。
- **[cfg_select](https://doc.rust-lang.org/stable/std/macro.cfg_select.html)** - `cfg` の条件に基づいてコンパイル時にコードを選択する。
- **[column](https://doc.rust-lang.org/stable/std/macro.column.html)** - 呼び出された列番号に展開される。
- **[compile_error](https://doc.rust-lang.org/stable/std/macro.compile_error.html)** - 遭遇した時点で、指定したエラーメッセージを伴いコンパイルを失敗させる。
- **[concat](https://doc.rust-lang.org/stable/std/macro.concat.html)** - リテラルを連結して静的な文字列スライスにする。
- **[dbg](https://doc.rust-lang.org/stable/std/macro.dbg.html)** - 手早く雑にデバッグするために、与えられた式の値を出力しつつ返す。
- **[debug_assert](https://doc.rust-lang.org/stable/std/macro.debug_assert.html)** - 実行時にブール式が `true` であることをアサートする。
- **[debug_assert_eq](https://doc.rust-lang.org/stable/std/macro.debug_assert_eq.html)** - 2つの式が等しいことをアサートする。
- **[debug_assert_matches](https://doc.rust-lang.org/stable/std/macro.debug_assert_matches.html)** - 式が与えられたパターンにマッチすることをアサートする。
- **[debug_assert_ne](https://doc.rust-lang.org/stable/std/macro.debug_assert_ne.html)** - 2つの式が等しくないことをアサートする。
- **[env](https://doc.rust-lang.org/stable/std/macro.env.html)** - コンパイル時に環境変数を検査する。
- **[eprint](https://doc.rust-lang.org/stable/std/macro.eprint.html)** - 標準エラー出力に出力する。
- **[eprintln](https://doc.rust-lang.org/stable/std/macro.eprintln.html)** - 標準エラー出力に、改行付きで出力する。
- **[file](https://doc.rust-lang.org/stable/std/macro.file.html)** - 呼び出されたファイル名に展開される。
- **[format](https://doc.rust-lang.org/stable/std/macro.format.html)** - 実行時の式の埋め込みを使って `String` を作成する。
- **[format_args](https://doc.rust-lang.org/stable/std/macro.format_args.html)** - 他の文字列整形マクロのためのパラメータを構築する。
- **[include](https://doc.rust-lang.org/stable/std/macro.include.html)** - コンテキストに応じて、ファイルを式または項目として解析する。
- **[include_bytes](https://doc.rust-lang.org/stable/std/macro.include_bytes.html)** - ファイルをバイト配列への参照として取り込む。
- **[include_str](https://doc.rust-lang.org/stable/std/macro.include_str.html)** - UTF-8 でエンコードされたファイルを文字列として取り込む。
- **[is_x86_feature_detected](https://doc.rust-lang.org/stable/std/macro.is_x86_feature_detected.html)** - 実行時に CPU 機能の有無を確認する。
- **[line](https://doc.rust-lang.org/stable/std/macro.line.html)** - 呼び出された行番号に展開される。
- **[matches](https://doc.rust-lang.org/stable/std/macro.matches.html)** - 与えられた式が指定したパターンにマッチするかどうかを返す。
- **[module_path](https://doc.rust-lang.org/stable/std/macro.module_path.html)** - 現在のモジュールパスを表す文字列に展開される。
- **[option_env](https://doc.rust-lang.org/stable/std/macro.option_env.html)** - コンパイル時に環境変数を任意で検査する。
- **[panic](https://doc.rust-lang.org/stable/std/macro.panic.html)** - 現在のスレッドをパニックさせる。
- **[print](https://doc.rust-lang.org/stable/std/macro.print.html)** - 標準出力に出力する。
- **[println](https://doc.rust-lang.org/stable/std/macro.println.html)** - 標準出力に、改行付きで出力する。
- **[stringify](https://doc.rust-lang.org/stable/std/macro.stringify.html)** - 引数を文字列化する。
- **[thread_local](https://doc.rust-lang.org/stable/std/macro.thread_local.html)** - [`std::thread::LocalKey`](https://doc.rust-lang.org/stable/std/thread/struct.LocalKey.html) 型の新しいスレッドローカルストレージキーを宣言する。
- **[todo](https://doc.rust-lang.org/stable/std/macro.todo.html)** - 未完成のコードを示す。
- **[try](https://doc.rust-lang.org/stable/std/macro.try.html)** - 結果をアンラップするか、そのエラーを伝播する。（非推奨）
- **[unimplemented](https://doc.rust-lang.org/stable/std/macro.unimplemented.html)** - 「not implemented」というメッセージでパニックすることにより、未実装のコードを示す。
- **[unreachable](https://doc.rust-lang.org/stable/std/macro.unreachable.html)** - 到達不能なコードを示す。
- **[vec](https://doc.rust-lang.org/stable/std/macro.vec.html)** - 引数を含む [`Vec`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html) を作成する。
- **[write](https://doc.rust-lang.org/stable/std/macro.write.html)** - 整形したデータをバッファに書き込む。
- **[writeln](https://doc.rust-lang.org/stable/std/macro.writeln.html)** - 整形したデータをバッファに、改行を追加して書き込む。
- **[concat_bytes](https://doc.rust-lang.org/stable/std/macro.concat_bytes.html)** - リテラルを連結してバイトスライスにする。（実験的）
- **[const_format_args](https://doc.rust-lang.org/stable/std/macro.const_format_args.html)** - [`format_args`](https://doc.rust-lang.org/stable/std/macro.format_args.html) と同じだが、一部の const コンテキストで使用できる。（実験的）
- **[hash_map](https://doc.rust-lang.org/stable/std/macro.hash_map.html)** - 引数を含む [`HashMap`](https://doc.rust-lang.org/stable/std/collections/struct.HashMap.html) を作成する。（実験的）
- **[log_syntax](https://doc.rust-lang.org/stable/std/macro.log_syntax.html)** - 渡されたトークンを標準出力に出力する。（実験的）
- **[trace_macros](https://doc.rust-lang.org/stable/std/macro.trace_macros.html)** - 他のマクロをデバッグするためのトレース機能を有効または無効にする。（実験的）

---

## キーワード

- **[SelfTy](https://doc.rust-lang.org/stable/std/keyword.SelfTy.html)** - [`trait`](https://doc.rust-lang.org/stable/std/keyword.trait.html) ブロックや [`impl`](https://doc.rust-lang.org/stable/std/keyword.impl.html) ブロックの中で実装対象となる型、あるいは型定義内での現在の型。
- **[as](https://doc.rust-lang.org/stable/std/keyword.as.html)** - 型間のキャスト、インポートのリネーム、関連項目へのパスの修飾。
- **[async](https://doc.rust-lang.org/stable/std/keyword.async.html)** - 現在のスレッドをブロックする代わりに [`Future`](https://doc.rust-lang.org/stable/std/future/trait.Future.html) を返す。
- **[await](https://doc.rust-lang.org/stable/std/keyword.await.html)** - [`Future`](https://doc.rust-lang.org/stable/std/future/trait.Future.html) の結果が準備できるまで実行を中断する。
- **[become](https://doc.rust-lang.org/stable/std/keyword.become.html)** - 関数の末尾呼び出し (tail call) を行う。
- **[break](https://doc.rust-lang.org/stable/std/keyword.break.html)** - ループやラベル付きブロックから早期に脱出する。
- **[const](https://doc.rust-lang.org/stable/std/keyword.const.html)** - コンパイル時定数、コンパイル時ブロック、コンパイル時に評価可能な関数、生ポインタ。
- **[continue](https://doc.rust-lang.org/stable/std/keyword.continue.html)** - ループの次のイテレーションへスキップする。
- **[crate](https://doc.rust-lang.org/stable/std/keyword.crate.html)** - Rust のバイナリまたはライブラリ。
- **[dyn](https://doc.rust-lang.org/stable/std/keyword.dyn.html)** - `dyn` は[トレイトオブジェクト](https://doc.rust-lang.org/stable/book/ch17-02-trait-objects.html)の型の接頭辞。
- **[else](https://doc.rust-lang.org/stable/std/keyword.else.html)** - [`if`](https://doc.rust-lang.org/stable/std/keyword.if.html) の条件が [`false`](https://doc.rust-lang.org/stable/std/keyword.false.html) と評価されたときに、どの式を評価するか。
- **[enum](https://doc.rust-lang.org/stable/std/keyword.enum.html)** - いくつかのバリアントのいずれかになれる型。
- **[extern](https://doc.rust-lang.org/stable/std/keyword.extern.html)** - 外部コードをリンクする、またはインポートする。
- **[false](https://doc.rust-lang.org/stable/std/keyword.false.html)** - 論理的な**偽**を表す [`bool`](https://doc.rust-lang.org/stable/std/primitive.bool.html) 型の値。
- **[fn](https://doc.rust-lang.org/stable/std/keyword.fn.html)** - 関数または関数ポインタ。
- **[for](https://doc.rust-lang.org/stable/std/keyword.for.html)** - [`in`](https://doc.rust-lang.org/stable/std/keyword.in.html) によるイテレーション、[`impl`](https://doc.rust-lang.org/stable/std/keyword.impl.html) によるトレイト実装、または[高階トレイト境界](https://doc.rust-lang.org/stable/reference/trait-bounds.html#higher-ranked-trait-bounds) (`for<'a>`)。
- **[if](https://doc.rust-lang.org/stable/std/keyword.if.html)** - 条件が成り立つ場合にブロックを評価する。
- **[impl](https://doc.rust-lang.org/stable/std/keyword.impl.html)** - 型に対する機能の実装、あるいは何らかの機能を実装する型。
- **[in](https://doc.rust-lang.org/stable/std/keyword.in.html)** - [`for`](https://doc.rust-lang.org/stable/std/keyword.for.html) で値の並びを反復する。
- **[let](https://doc.rust-lang.org/stable/std/keyword.let.html)** - 変数に値を束縛する。
- **[loop](https://doc.rust-lang.org/stable/std/keyword.loop.html)** - 無限にループする。
- **[match](https://doc.rust-lang.org/stable/std/keyword.match.html)** - パターンマッチングに基づく制御フロー。
- **[mod](https://doc.rust-lang.org/stable/std/keyword.mod.html)** - コードを[モジュール](https://doc.rust-lang.org/stable/reference/items/modules.html)に組織化する。
- **[move](https://doc.rust-lang.org/stable/std/keyword.move.html)** - [クロージャ](https://doc.rust-lang.org/stable/book/ch13-01-closures.html)の環境を値としてキャプチャする。
- **[mut](https://doc.rust-lang.org/stable/std/keyword.mut.html)** - 可変な変数、参照、ポインタ。
- **[pub](https://doc.rust-lang.org/stable/std/keyword.pub.html)** - 項目を他から見えるようにする。
- **[ref](https://doc.rust-lang.org/stable/std/keyword.ref.html)** - パターンマッチングの際に参照として束縛する。
- **[return](https://doc.rust-lang.org/stable/std/keyword.return.html)** - 関数から値を返す。
- **[self](https://doc.rust-lang.org/stable/std/keyword.self.html)** - メソッドの受け手、または現在のモジュール。
- **[static](https://doc.rust-lang.org/stable/std/keyword.static.html)** - static 項目は、プログラム全体の間有効な値 (`'static` ライフタイム)。
- **[struct](https://doc.rust-lang.org/stable/std/keyword.struct.html)** - 他の型を組み合わせて構成される型。
- **[super](https://doc.rust-lang.org/stable/std/keyword.super.html)** - 現在の[モジュール](https://doc.rust-lang.org/stable/reference/items/modules.html)の親。
- **[trait](https://doc.rust-lang.org/stable/std/keyword.trait.html)** - 一群の型に共通のインタフェース。
- **[true](https://doc.rust-lang.org/stable/std/keyword.true.html)** - 論理的な**真**を表す [`bool`](https://doc.rust-lang.org/stable/std/primitive.bool.html) 型の値。
- **[type](https://doc.rust-lang.org/stable/std/keyword.type.html)** - 既存の型に対する[エイリアス](https://doc.rust-lang.org/stable/reference/items/type-aliases.html)を定義する。
- **[union](https://doc.rust-lang.org/stable/std/keyword.union.html)** - [C 言語の union に相当する Rust の仕組み](https://doc.rust-lang.org/stable/reference/items/unions.html)。
- **[unsafe](https://doc.rust-lang.org/stable/std/keyword.unsafe.html)** - 型システムによって[メモリ安全性](https://doc.rust-lang.org/stable/book/ch19-01-unsafe-rust.html)を検証できないコードやインタフェース。
- **[use](https://doc.rust-lang.org/stable/std/keyword.use.html)** - 他のクレートやモジュールから項目をインポートまたはリネームする、人間工学的なクローンのセマンティクスで値を使う、あるいは `use<..>` で正確なキャプチャを指定する。
- **[where](https://doc.rust-lang.org/stable/std/keyword.where.html)** - 項目を使うために満たさなければならない制約を追加する。
- **[while](https://doc.rust-lang.org/stable/std/keyword.while.html)** - 条件が成り立つ間ループする。

---

## 属性マクロ

- **[derive](https://doc.rust-lang.org/stable/std/attr.derive.html)** - derive マクロを適用するための属性マクロ。

---

## 属性

- **[allow](https://doc.rust-lang.org/stable/std/attribute.allow.html)** - `allow` 属性は、そうでなければ警告やエラーを発生させる lint の診断を抑制します。（[`forbid`](https://doc.rust-lang.org/stable/std/attribute.forbid.html) に設定されたものを除き）任意の lint または lint グループに使用できます。
- **[cfg](https://doc.rust-lang.org/stable/std/attribute.cfg.html)** - 条件付きコンパイルに使用します。
- **[cold](https://doc.rust-lang.org/stable/std/attribute.cold.html)** - 関数が呼び出される可能性が低いことをコンパイラに示唆します。
- **[deny](https://doc.rust-lang.org/stable/std/attribute.deny.html)** - lint チェックが失敗したときにエラーを発生させ、コンパイルの完了を妨げます。これはルールを強制したり、特定のパターンを防止したりするのに有用です。
- **[deprecated](https://doc.rust-lang.org/stable/std/attribute.deprecated.html)** - この属性が付いた項目が使用されたときに、コンパイル中に警告を発します。`since` と `note` は、その項目が非推奨である理由をより詳しく示す任意のフィールドです。
- **[forbid](https://doc.rust-lang.org/stable/std/attribute.forbid.html)** - lint チェックが失敗したときにエラーを発生させ、コンパイルの完了を妨げます。
- **[inline](https://doc.rust-lang.org/stable/std/attribute.inline.html)** - 呼び出し箇所で関数をインライン化するようコンパイラに提案します。
- **[link_section](https://doc.rust-lang.org/stable/std/attribute.link_section.html)** - 関数や static を特定のオブジェクトファイルのセクションに配置します。
- **[must_use](https://doc.rust-lang.org/stable/std/attribute.must_use.html)** - 値が無視されたときに警告します。
- **[no_std](https://doc.rust-lang.org/stable/std/attribute.no_std.html)** - 標準ライブラリの自動リンクを防止します。
- **[non_exhaustive](https://doc.rust-lang.org/stable/std/attribute.non_exhaustive.html)** - 将来フィールドやバリアントが追加される可能性がある型であることを示します。
- **[proc_macro](https://doc.rust-lang.org/stable/std/attribute.proc_macro.html)** - 関数型の手続き的マクロ (proc macro) を定義します。
- **[track_caller](https://doc.rust-lang.org/stable/std/attribute.track_caller.html)** - 関数が、自身の場所ではなく呼び出し元の場所を報告するようにします。
- **[warn](https://doc.rust-lang.org/stable/std/attribute.warn.html)** - lint チェックが失敗したときに、コンパイル中に警告を発します。

---

本ページは [`std` (stable)](https://doc.rust-lang.org/stable/std/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
