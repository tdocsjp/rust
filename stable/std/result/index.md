---
title: result
---

# Module result

`Result` 型によるエラーハンドリング。

[`Result<T, E>`](./enum.Result.md) は、エラーを返したり伝播させたりするために使われる型です。これは、成功を表し値を含む [`Ok(T)`](./enum.Result.md#variants.Ok) と、エラーを表しエラー値を含む [`Err(E)`](./enum.Result.md#variants.Err) というバリアントを持つ enum です。

```rust
enum Result<T, E> {
   Ok(T),
   Err(E),
}
```

エラーが予期され、かつ回復可能である場合、関数は [`Result`](./enum.Result.md) を返します。`std` クレートでは、[`Result`](./enum.Result.md) は特に [I/O](/stable/std/io/) で使われることが多いです。

[`Result`](./enum.Result.md) を返す単純な関数は、次のように定義・使用できます。

```rust
#[derive(Debug)]
enum Version { Version1, Version2 }

fn parse_version(header: &[u8]) -> Result<Version, &'static str> {
    match header.get(0) {
        None => Err("invalid header length"),
        Some(&1) => Ok(Version::Version1),
        Some(&2) => Ok(Version::Version2),
        Some(_) => Err("invalid version"),
    }
}

let version = parse_version(&[1, 2, 3, 4]);
match version {
    Ok(v) => println!("working with version: {v:?}"),
    Err(e) => println!("error parsing header: {e:?}"),
}
```

単純な場合には [`Result`](./enum.Result.md) に対するパターンマッチングは明快で分かりやすいですが、[`Result`](./enum.Result.md) にはそれをより簡潔に扱うための便利なメソッドがいくつも用意されています。

```rust
// `is_ok` と `is_err` メソッドは、その名前が示す通りのことをする。
let good_result: Result<i32, i32> = Ok(10);
let bad_result: Result<i32, i32> = Err(10);
assert!(good_result.is_ok() && !good_result.is_err());
assert!(bad_result.is_err() && !bad_result.is_ok());

// `map` と `map_err` は `Result` を消費し、別の `Result` を生成する。
let good_result: Result<i32, i32> = good_result.map(|i| i + 1);
let bad_result: Result<i32, i32> = bad_result.map_err(|i| i - 1);
assert_eq!(good_result, Ok(11));
assert_eq!(bad_result, Err(9));

// `and_then` を使って計算を続ける。
let good_result: Result<bool, i32> = good_result.and_then(|i| Ok(i == 11));
assert_eq!(good_result, Ok(true));

// `or_else` を使ってエラーを処理する。
let bad_result: Result<i32, i32> = bad_result.or_else(|i| Ok(i + 20));
assert_eq!(bad_result, Ok(29));

// `unwrap` で result を消費し、内容を取り出す。
let final_awesome_result = good_result.unwrap();
assert!(final_awesome_result)
```

## Result は使われなければならない

戻り値でエラーを示す際によくある問題は、戻り値を無視するのが簡単で、その結果エラー処理を怠ってしまうことです。[`Result`](./enum.Result.md) には `#[must_use]` 属性が付いており、Result の値が無視されるとコンパイラが警告を出すようになっています。これにより、[`Result`](./enum.Result.md) はエラーに遭遇する可能性はあるが、それ以外には有用な値を返さない関数で特に役立ちます。

[`Write`](/stable/std/io/) トレイトが I/O 型向けに定義している [`write_all`](/stable/std/io/) メソッドを考えてみましょう。

```rust
use std::io;

trait Write {
    fn write_all(&mut self, bytes: &[u8]) -> Result<(), io::Error>;
}
```

_注意: 実際の [`Write`](/stable/std/io/) の定義では、単に `Result<T, io::Error>` の別名である [`io::Result`](/stable/std/io/) が使われています。_

このメソッドは値を生成しませんが、書き込みが失敗する可能性があります。エラーの場合を処理することが重要であり、次のようなコードを書いては_いけません_。

```rust
use std::fs::File;
use std::io::prelude::*;

let mut file = File::create("valuable_data.txt").unwrap();
// `write_all` がエラーになっても、戻り値が無視されているので、
// 私たちはそれを知ることができない。
file.write_all(b"important message");
```

Rust で実際にそう書くと、コンパイラが警告を出します（デフォルトでは `unused_must_use` lint によって制御されます）。

エラーを処理したくない場合は、代わりに [`expect`](./enum.Result.md) で成功を単純にアサートするとよいでしょう。これは書き込みが失敗した場合にパニックし、なぜ書き込みが成功するはずだったのかを説明するメッセージを提供します。

```rust
use std::fs::File;
use std::io::prelude::*;

let mut file = File::create("valuable_data.txt").unwrap();
file.write_all(b"important message").expect("writing to the file should succeed");
```

単純に成功をアサートすることもできます。

```rust
assert!(file.write_all(b"important message").is_ok());
```

あるいは、[`?`](/stable/std/ops/) でエラーを呼び出し元へ伝播させることもできます。

```rust
fn write_message() -> io::Result<()> {
    let mut file = File::create("valuable_data.txt")?;
    file.write_all(b"important message")?;
    Ok(())
}
```

## `?` 演算子（question mark operator）

[`Result`](./enum.Result.md) 型を返す多くの関数を呼び出すコードを書くとき、エラー処理は煩雑になりがちです。`?` 演算子は、呼び出し元へエラーを伝播させる定型コードの一部を隠してくれます。

これは次のコードを:

```rust
use std::fs::File;
use std::io::prelude::*;
use std::io;

struct Info {
    name: String,
    age: i32,
    rating: i32,
}

fn write_info(info: &Info) -> io::Result<()> {
    // エラー時に早期リターン
    let mut file = match File::create("my_best_friends.txt") {
           Err(e) => return Err(e),
           Ok(f) => f,
    };
    if let Err(e) = file.write_all(format!("name: {}\n", info.name).as_bytes()) {
        return Err(e)
    }
    if let Err(e) = file.write_all(format!("age: {}\n", info.age).as_bytes()) {
        return Err(e)
    }
    if let Err(e) = file.write_all(format!("rating: {}\n", info.rating).as_bytes()) {
        return Err(e)
    }
    Ok(())
}
```

次のように置き換えます:

```rust
use std::fs::File;
use std::io::prelude::*;
use std::io;

struct Info {
    name: String,
    age: i32,
    rating: i32,
}

fn write_info(info: &Info) -> io::Result<()> {
    let mut file = File::create("my_best_friends.txt")?;
    // エラー時に早期リターン
    file.write_all(format!("name: {}\n", info.name).as_bytes())?;
    file.write_all(format!("age: {}\n", info.age).as_bytes())?;
    file.write_all(format!("rating: {}\n", info.rating).as_bytes())?;
    Ok(())
}
```

_ずっと綺麗になりました！_

式を `?` で終えると、結果が [`Err`](./enum.Result.md) でない限り、[`Ok`](./enum.Result.md) の中身がアンラップされた値になります。結果が [`Err`](./enum.Result.md) の場合は、それを囲む関数から早期に [`Err`](./enum.Result.md) が返されます。

`?` が提供する [`Err`](./enum.Result.md) の早期リターンのため、`?` は [`Result`](./enum.Result.md) を返す関数の中で使うことができます。

## 表現（Representation）

一部のケースでは、[`Result<T, E>`](./enum.Result.md) にはサイズ・アライメント・ABI に関する保証があります。具体的には、`T` または `E` のいずれか一方の型が `Option` の[表現の保証](/stable/std/option/#表現representation)を満たす型（この型を `I` と呼びます）であり、_もう一方_ の型がアライメント1のゼロサイズ型（「1-ZST」）である必要があります。

その場合、`Result<T, E>` は `I`（したがって `Option<I>`）と同じサイズ・アライメント・[関数呼び出しの ABI](https://doc.rust-lang.org/stable/std/primitive.fn.html#abi-compatibility) を持ちます。`I` が `T` であれば、したがって型 `I` の値 `t` を型 `Result<T, E>` へ transmute すること（結果として値 `Ok(t)` を得る）、および型 `Result<T, E>` の値 `Ok(t)` を型 `I` へ transmute すること（結果として値 `t` を得る）は健全です。`I` が `E` であれば、`Ok` を `Err` に置き換えた上で同じことが当てはまります。

たとえば、`NonZeroI32` は `Option` の表現の保証を満たし、`()` はアライメント1のゼロサイズ型です。つまり `Result<NonZeroI32, ()>` と `Result<(), NonZeroI32>` はどちらも `NonZeroI32`（および `Option<NonZeroI32>`）と同じサイズ・アライメント・ABI を持ちます。これらの違いは、暗示される意味だけです。

* `Option<NonZeroI32>` は「ゼロでない i32 が存在するかもしれない」
* `Result<NonZeroI32, ()>` は「ゼロでない i32 の成功結果が、あれば」
* `Result<(), NonZeroI32>` は「ゼロでない i32 のエラー結果が、あれば」

## メソッド概観

パターンマッチングで扱うことに加えて、[`Result`](./enum.Result.md) は多種多様なメソッドを提供します。

### バリアントの問い合わせ

[`is_ok`](./enum.Result.md) メソッドと [`is_err`](./enum.Result.md) メソッドは、[`Result`](./enum.Result.md) がそれぞれ [`Ok`](./enum.Result.md) または [`Err`](./enum.Result.md) であるとき `true` を返します。

[`is_ok_and`](./enum.Result.md) メソッドと [`is_err_and`](./enum.Result.md) メソッドは、与えられた関数を [`Result`](./enum.Result.md) の内容に適用して真偽値を生成します。[`Result`](./enum.Result.md) が期待したバリアントでない場合は、関数を実行せずに `false` が返されます。

### 参照を扱うためのアダプタ

* [`as_ref`](./enum.Result.md) は `&Result<T, E>` から `Result<&T, &E>` へ変換する
* [`as_mut`](./enum.Result.md) は `&mut Result<T, E>` から `Result<&mut T, &mut E>` へ変換する
* [`as_deref`](./enum.Result.md) は `&Result<T, E>` から `Result<&T::Target, &E>` へ変換する
* [`as_deref_mut`](./enum.Result.md) は `&mut Result<T, E>` から `Result<&mut T::Target, &mut E>` へ変換する

### 含まれている値の取り出し

これらのメソッドは、[`Result<T, E>`](./enum.Result.md) が [`Ok`](./enum.Result.md) バリアントであるとき、含まれている値を取り出します。[`Result`](./enum.Result.md) が [`Err`](./enum.Result.md) の場合:

* [`expect`](./enum.Result.md) は与えられたカスタムメッセージでパニックする
* [`unwrap`](./enum.Result.md) は汎用的なメッセージでパニックする
* [`unwrap_or`](./enum.Result.md) は与えられたデフォルト値を返す
* [`unwrap_or_default`](./enum.Result.md) は型 `T` のデフォルト値を返す（`T` は [`Default`](/stable/std/default/) トレイトを実装していなければならない）
* [`unwrap_or_else`](./enum.Result.md) は与えられた関数を評価した結果を返す
* [`unwrap_unchecked`](./enum.Result.md) は_未定義動作_を引き起こす

パニックするメソッドである [`expect`](./enum.Result.md) と [`unwrap`](./enum.Result.md) は、`E` が [`Debug`](/stable/std/fmt/) トレイトを実装していることを要求します。

これらのメソッドは、[`Result<T, E>`](./enum.Result.md) が [`Err`](./enum.Result.md) バリアントであるとき、含まれている値を取り出します。これらは `T` が [`Debug`](/stable/std/fmt/) トレイトを実装していることを要求します。[`Result`](./enum.Result.md) が [`Ok`](./enum.Result.md) の場合:

* [`expect_err`](./enum.Result.md) は与えられたカスタムメッセージでパニックする
* [`unwrap_err`](./enum.Result.md) は汎用的なメッセージでパニックする
* [`unwrap_err_unchecked`](./enum.Result.md) は_未定義動作_を引き起こす

### 含まれている値の変換

これらのメソッドは [`Result`](./enum.Result.md) を [`Option`](/stable/std/option/) へ変換します。

* [`err`](./enum.Result.md) は [`Result<T, E>`](./enum.Result.md) を [`Option<E>`](/stable/std/option/) へ変換し、[`Err(e)`](./enum.Result.md) を [`Some(e)`](/stable/std/option/) へ、[`Ok(v)`](./enum.Result.md) を [`None`](/stable/std/option/) へ対応させる
* [`ok`](./enum.Result.md) は [`Result<T, E>`](./enum.Result.md) を [`Option<T>`](/stable/std/option/) へ変換し、[`Ok(v)`](./enum.Result.md) を [`Some(v)`](/stable/std/option/) へ、[`Err(e)`](./enum.Result.md) を [`None`](/stable/std/option/) へ対応させる
* [`transpose`](./enum.Result.md) は [`Option`](/stable/std/option/) の [`Result`](./enum.Result.md) を、[`Result`](./enum.Result.md) の [`Option`](/stable/std/option/) へ転置する

これらのメソッドは [`Ok`](./enum.Result.md) バリアントの含まれている値を変換します。

* [`map`](./enum.Result.md) は、[`Ok`](./enum.Result.md) の含まれている値に与えられた関数を適用し、[`Err`](./enum.Result.md) の値はそのままにすることで [`Result<T, E>`](./enum.Result.md) を [`Result<U, E>`](./enum.Result.md) へ変換する
* [`inspect`](./enum.Result.md) は [`Result`](./enum.Result.md) の所有権を取り、含まれている値への参照に与えられた関数を適用し、その後 [`Result`](./enum.Result.md) を返す

これらのメソッドは [`Err`](./enum.Result.md) バリアントの含まれている値を変換します。

* [`map_err`](./enum.Result.md) は、[`Err`](./enum.Result.md) の含まれている値に与えられた関数を適用し、[`Ok`](./enum.Result.md) の値はそのままにすることで [`Result<T, E>`](./enum.Result.md) を [`Result<T, F>`](./enum.Result.md) へ変換する
* [`inspect_err`](./enum.Result.md) は [`Result`](./enum.Result.md) の所有権を取り、[`Err`](./enum.Result.md) の含まれている値への参照に与えられた関数を適用し、その後 [`Result`](./enum.Result.md) を返す

これらのメソッドは [`Result<T, E>`](./enum.Result.md) を、場合によっては異なる型 `U` の値へ変換します。

* [`map_or`](./enum.Result.md) は [`Ok`](./enum.Result.md) の含まれている値に与えられた関数を適用する。[`Result`](./enum.Result.md) が [`Err`](./enum.Result.md) の場合は与えられたデフォルト値を返す
* [`map_or_else`](./enum.Result.md) は [`Ok`](./enum.Result.md) の含まれている値に与えられた関数を適用する。[`Err`](./enum.Result.md) の含まれている値には与えられたフォールバック関数を適用する

### 真偽値演算子

これらのメソッドは [`Result`](./enum.Result.md) を真偽値として扱い、[`Ok`](./enum.Result.md) は `true` のように、[`Err`](./enum.Result.md) は `false` のように振る舞います。これらのメソッドには2種類あります。1つは [`Result`](./enum.Result.md) を入力に取るもの、もう1つは（遅延評価される）関数を入力に取るものです。

[`and`](./enum.Result.md) メソッドと [`or`](./enum.Result.md) メソッドは別の [`Result`](./enum.Result.md) を入力に取り、[`Result`](./enum.Result.md) を出力として生成します。[`and`](./enum.Result.md) メソッドは、[`Result<T, E>`](./enum.Result.md) とは異なる内側の型 `U` を持つ [`Result<U, E>`](./enum.Result.md) の値を生成できます。[`or`](./enum.Result.md) メソッドは、[`Result<T, E>`](./enum.Result.md) とは異なるエラー型 `F` を持つ [`Result<T, F>`](./enum.Result.md) の値を生成できます。

| メソッド | self | 入力 | 出力 |
|--------|------|-------|--------|
| `and` | `Err(e)` | （無視される） | `Err(e)` |
| `and` | `Ok(x)` | `Err(d)` | `Err(d)` |
| `and` | `Ok(x)` | `Ok(y)` | `Ok(y)` |
| `or` | `Err(e)` | `Err(d)` | `Err(d)` |
| `or` | `Err(e)` | `Ok(y)` | `Ok(y)` |
| `or` | `Ok(x)` | （無視される） | `Ok(x)` |

[`and_then`](./enum.Result.md) メソッドと [`or_else`](./enum.Result.md) メソッドは関数を入力に取り、新しい値を生成する必要があるときにだけその関数を評価します。[`and_then`](./enum.Result.md) メソッドは、[`Result<T, E>`](./enum.Result.md) とは異なる内側の型 `U` を持つ [`Result<U, E>`](./enum.Result.md) の値を生成できます。[`or_else`](./enum.Result.md) メソッドは、[`Result<T, E>`](./enum.Result.md) とは異なるエラー型 `F` を持つ [`Result<T, F>`](./enum.Result.md) の値を生成できます。

| メソッド | self | 関数の入力 | 関数の結果 | 出力 |
|--------|------|-----------------|-----------------|--------|
| `and_then` | `Err(e)` | （渡されない） | （評価されない） | `Err(e)` |
| `and_then` | `Ok(x)` | `x` | `Err(d)` | `Err(d)` |
| `and_then` | `Ok(x)` | `x` | `Ok(y)` | `Ok(y)` |
| `or_else` | `Err(e)` | `e` | `Err(d)` | `Err(d)` |
| `or_else` | `Err(e)` | `e` | `Ok(y)` | `Ok(y)` |
| `or_else` | `Ok(x)` | （渡されない） | （評価されない） | `Ok(x)` |

### 比較演算子

`T` と `E` の両方が [`PartialOrd`](/stable/std/cmp/) を実装していれば、[`Result<T, E>`](./enum.Result.md) はその [`PartialOrd`](/stable/std/cmp/) 実装を導出します。この順序では、[`Ok`](./enum.Result.md) はどの [`Err`](./enum.Result.md) よりも小さいと比較され、2つの [`Ok`](./enum.Result.md) または2つの [`Err`](./enum.Result.md) は、それぞれ `T` または `E` において含まれている値が比較される方法と同じように比較されます。`T` と `E` の両方が [`Ord`](/stable/std/cmp/) も実装していれば、[`Result<T, E>`](./enum.Result.md) もそうなります。

```rust
assert!(Ok(1) < Err(0));
let x: Result<i32, ()> = Ok(0);
let y = Ok(1);
assert!(x < y);
let x: Result<(), i32> = Err(0);
let y = Err(1);
assert!(x < y);
```

### `Result` に対する反復

[`Result`](./enum.Result.md) は反復できます。これは、条件によって空になるイテレータが必要な場合に役立ちます。このイテレータは（[`Result`](./enum.Result.md) が [`Ok`](./enum.Result.md) のとき）単一の値を生成するか、（[`Result`](./enum.Result.md) が [`Err`](./enum.Result.md) のとき）値を生成しません。たとえば、[`into_iter`](./enum.Result.md) は [`Result`](./enum.Result.md) が [`Ok(v)`](./enum.Result.md) のとき [`once(v)`](/stable/std/iter/) のように振る舞い、[`Err`](./enum.Result.md) のとき [`empty()`](/stable/std/iter/) のように振る舞います。

[`Result<T, E>`](./enum.Result.md) に対するイテレータには3種類あります。

* [`into_iter`](./enum.Result.md) は [`Result`](./enum.Result.md) を消費し、含まれている値を生成する
* [`iter`](./enum.Result.md) は含まれている値への不変参照 `&T` を生成する
* [`iter_mut`](./enum.Result.md) は含まれている値への可変参照 `&mut T` を生成する

これがどのように役立つかの例は、[`Option` に対する反復](/stable/std/option/#option-に対する反復)を参照してください。

失敗しうる操作を何度も行いたいが、処理を続ける間は失敗を無視して成功した結果だけを扱いたい、という場合にイテレータ連結を使いたくなるかもしれません。この例では、[`Result`](./enum.Result.md) が反復可能であることを利用し、[`flatten`](/stable/std/iter/) を使って [`Ok`](./enum.Result.md) の値だけを選んでいます。

```rust
let mut results = vec![];
let mut errs = vec![];
let nums: Vec<_> = ["17", "not a number", "99", "-27", "768"]
   .into_iter()
   .map(u8::from_str)
   // 検証用に生の `Result` 値のクローンを保存する
   .inspect(|x| results.push(x.clone()))
   // 課題: これがどのように `Err` の値だけを捉えているか説明してみよう
   .inspect(|x| errs.extend(x.clone().err()))
   .flatten()
   .collect();
assert_eq!(errs.len(), 3);
assert_eq!(nums, [17, 99]);
println!("results {results:?}");
println!("errs {errs:?}");
println!("nums {nums:?}");
```

### `Result` への収集

[`Result`](./enum.Result.md) は `FromIterator` トレイトを実装しています。これにより、[`Result`](./enum.Result.md) 値に対するイテレータを、元の [`Result`](./enum.Result.md) 値それぞれに含まれていた値のコレクションを持つ [`Result`](./enum.Result.md) へ、あるいは要素のいずれかが [`Err`](./enum.Result.md) であれば [`Err`](./enum.Result.md) へ、収集できます。

```rust
let v = [Ok(2), Ok(4), Err("err!"), Ok(8)];
let res: Result<Vec<_>, &str> = v.into_iter().collect();
assert_eq!(res, Err("err!"));
let v = [Ok(2), Ok(4), Ok(8)];
let res: Result<Vec<_>, &str> = v.into_iter().collect();
assert_eq!(res, Ok(vec![2, 4, 8]));
```

[`Result`](./enum.Result.md) は `Product` トレイトと `Sum` トレイトも実装しているため、[`Result`](./enum.Result.md) 値に対するイテレータは [`product`](/stable/std/iter/) メソッドと [`sum`](/stable/std/iter/) メソッドを使えます。

```rust
let v = [Err("error!"), Ok(1), Ok(2), Ok(3), Err("foo")];
let res: Result<i32, &str> = v.into_iter().sum();
assert_eq!(res, Err("error!"));
let v = [Ok(1), Ok(2), Ok(21)];
let res: Result<i32, &str> = v.into_iter().product();
assert_eq!(res, Ok(42));
```

## 構造体

[IntoIter](struct.IntoIter.html)

[`Result`](./enum.Result.md) の [`Ok`](./enum.Result.md) バリアントの値に対するイテレータ。

[Iter](struct.Iter.html)

[`Result`](./enum.Result.md) の [`Ok`](./enum.Result.md) バリアントへの参照に対するイテレータ。

[IterMut](struct.IterMut.html)

[`Result`](./enum.Result.md) の [`Ok`](./enum.Result.md) バリアントへの可変参照に対するイテレータ。

## 列挙型

[Result](./enum.Result.md)

`Result` は、成功 ([`Ok`](./enum.Result.md)) または失敗 ([`Err`](./enum.Result.md)) のいずれかを表す型です。

---

本ページは [`std::result` (stable)](https://doc.rust-lang.org/stable/std/result/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
