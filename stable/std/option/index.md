---
title: option
---

# Module option

オプショナルな値。

`Option` 型はオプショナルな値を表します。すべての `Option` は `Some` であって値を含んでいるか、`None` であって値を含んでいないかのいずれかです。`Option` 型は Rust のコードで非常によく使われます。その用途には次のようなものがあります。

* 初期値
* 入力の全範囲に対して定義されていない関数（部分関数）の戻り値
* それ以外の方法で単純なエラーを報告する戻り値。エラー時に `None` を返す
* オプショナルな構造体フィールド
* 借用したり「取り出したり (take)」できる構造体フィールド
* オプショナルな関数の引数
* null になりうるポインタ
* 困難な状況から何かを取り出す（swap する）

`Option` はパターンマッチングと組み合わせて、値の有無を問い合わせ、それに応じて行動する（常に `None` の場合を考慮する）ために使われるのが一般的です。

```rust
fn divide(numerator: f64, denominator: f64) -> Option<f64> {
    if denominator == 0.0 {
        None
    } else {
        Some(numerator / denominator)
    }
}

// 関数の戻り値は Option になる
let result = divide(2.0, 3.0);

// パターンマッチで値を取り出す
match result {
    // 除算は有効だった
    Some(x) => println!("Result: {x}"),
    // 除算は無効だった
    None    => println!("Cannot divide by 0"),
}
```

## Option とポインタ（「null になりうる」ポインタ）

Rust のポインタ型は常に有効な場所を指していなければなりません。「null」な参照は存在しません。代わりに、Rust には、たとえば `Option<Box<T>>` のようなオプショナルな所有ボックスといった、_オプショナルな_ポインタがあります。

次の例では `Option` を使って、`i32` のオプショナルなボックスを作成しています。内側の `i32` の値を使うには、`check_optional` 関数がまず、そのボックスが値を持っている（つまり `Some(...)` である）かどうかをパターンマッチングで判定する必要があることに注意してください。

```rust
let optional = None;
check_optional(optional);

let optional = Some(Box::new(9000));
check_optional(optional);

fn check_optional(optional: Option<Box<i32>>) {
    match optional {
        Some(p) => println!("has value {p}"),
        None => println!("has no value"),
    }
}
```

## `?` 演算子（question mark operator）

`Result` 型と同様に、`Option` 型を返す多くの関数を呼び出すコードを書く際には、`Some`/`None` の処理が煩雑になりがちです。`?` 演算子は、呼び出し元へ値を伝播させる定型コードの一部を隠してくれます。

これは次のコードを:

```rust
fn add_last_numbers(stack: &mut Vec<i32>) -> Option<i32> {
    let a = stack.pop();
    let b = stack.pop();

    match (a, b) {
        (Some(x), Some(y)) => Some(x + y),
        _ => None,
    }
}
```

次のように置き換えます:

```rust
fn add_last_numbers(stack: &mut Vec<i32>) -> Option<i32> {
    Some(stack.pop()? + stack.pop()?)
}
```

_ずっと綺麗になりました！_

式を `?` で終えると、結果が `None` でない限り、`Some` の中身がアンラップされた値になります。結果が `None` の場合は、それを囲む関数から早期に `None` が返されます。

`?` が提供する `None` の早期リターンのため、`?` は `Option` を返す関数の中で使うことができます。

## 表現（Representation）

Rust は、次のような型 `T` に対して、`Option<T>` が `T` と同じサイズ・アライメント・[関数呼び出しの ABI](https://doc.rust-lang.org/stable/std/primitive.fn.html#abi-compatibility) を持つように最適化することを保証しています。したがって、`T` がこれらの型のいずれかである場合、型 `T` の値 `t` を型 `Option<T>` へ transmute すること（結果として値 `Some(t)` を得る）、および型 `Option<T>` の値 `Some(t)` を型 `T` へ transmute すること（結果として値 `t` を得る）は、健全です。

これらの型の一部では、Rust はさらに次を保証します。

* `transmute::<_, Option<T>>([0u8; size_of::<T>()])` は健全であり、`Option::<T>::None` を生成する
* `transmute::<_, [u8; size_of::<T>()]>(Option::<T>::None)` は健全であり、`[0u8; size_of::<T>()]` を生成する

これらのケースは2列目で示されています。

| `T` | `[0u8; size_of::<T>()]` と `Option::<T>::None` の間での transmute が健全か |
|-----|------------|
| [`Box<U>`](https://doc.rust-lang.org/stable/std/boxed/struct.Box.html)（具体的には `Box<U, Global>` のみ） | `U: Sized` のとき |
| `&U` | `U: Sized` のとき |
| `&mut U` | `U: Sized` のとき |
| `fn`、`extern "C" fn` | 常に |
| [`num::NonZero*`](https://doc.rust-lang.org/stable/core/num/index.html) | 常に |
| [`ptr::NonNull<U>`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html) | `U: Sized` のとき |
| このリストにある型のいずれかを包む `#[repr(transparent)]` 構造体 | 内側の型について成り立つとき |

一部の条件下では、上記の型 `T` は `Result` に包まれた場合にも null ポインタ最適化されます。

これは「null ポインタ最適化（null pointer optimization、NPO）」と呼ばれます。

さらに、上記のケースにおいては、`T` のすべての正当な値から `Option<T>` へ、そして `Some::<T>(_)` から `T` へ [`mem::transmute`](https://doc.rust-lang.org/stable/std/mem/fn.transmute.html) できることが保証されています（しかし `None::<T>` を `T` へ transmute するのは未定義動作です）。

## メソッド概観

パターンマッチングで扱うことに加えて、`Option` は多種多様なメソッドを提供します。

### バリアントの問い合わせ

`is_some` メソッドと `is_none` メソッドは、`Option` がそれぞれ `Some` または `None` であるとき `true` を返します。

`is_some_and` メソッドと `is_none_or` メソッドは、与えられた関数を `Option` の内容に適用して真偽値を生成します。`None` の場合は関数を実行せずにデフォルトの結果を返します。

### 参照を扱うためのアダプタ

* `as_ref` は `&Option<T>` から `Option<&T>` へ変換する
* `as_mut` は `&mut Option<T>` から `Option<&mut T>` へ変換する
* `as_deref` は `&Option<T>` から `Option<&T::Target>` へ変換する
* `as_deref_mut` は `&mut Option<T>` から `Option<&mut T::Target>` へ変換する
* `as_pin_ref` は `Pin<&Option<T>>` から `Option<Pin<&T>>` へ変換する
* `as_pin_mut` は `Pin<&mut Option<T>>` から `Option<Pin<&mut T>>` へ変換する
* `as_slice` は、含まれている値があればその1要素のスライスを返す。`None` の場合は空のスライスを返す
* `as_mut_slice` は、含まれている値があればその1要素の可変スライスを返す。`None` の場合は空のスライスを返す

### 含まれている値の取り出し

これらのメソッドは、`Option<T>` が `Some` バリアントであるとき、含まれている値を取り出します。`Option` が `None` の場合:

* `expect` は与えられたカスタムメッセージでパニックする
* `unwrap` は汎用的なメッセージでパニックする
* `unwrap_or` は与えられたデフォルト値を返す
* `unwrap_or_default` は型 `T` のデフォルト値を返す（`T` は `Default` トレイトを実装していなければならない）
* `unwrap_or_else` は与えられた関数を評価した結果を返す
* `unwrap_unchecked` は _未定義動作_ を引き起こす

### 含まれている値の変換

これらのメソッドは `Option` を `Result` へ変換します。

* `ok_or` は `Some(v)` を `Ok(v)` へ、`None` を与えられたデフォルトの `err` 値を使って `Err(err)` へ変換する
* `ok_or_else` は `Some(v)` を `Ok(v)` へ、`None` を与えられた関数を使った `Err` の値へ変換する
* `transpose` は `Result` の `Option` を `Option` の `Result` へ転置する

これらのメソッドは `Some` バリアントを変換します。

* `filter` は、`Option` が `Some(t)` であれば含まれている値 `t` に対して与えられた述語関数を呼び出し、関数が `true` を返せば `Some(t)` を返す。そうでなければ `None` を返す
* `flatten` は `Option<Option<T>>` から入れ子を1段階取り除く
* `inspect` メソッドは `Option` の所有権を取り、`Some` であれば含まれている値への参照に対して与えられた関数を適用する
* `map` は、`Some` の含まれている値に与えられた関数を適用することで `Option<T>` を `Option<U>` へ変換し、`None` の値はそのままにする

これらのメソッドは `Option<T>` を、場合によっては異なる型 `U` の値へ変換します。

* `map_or` は `Some` の含まれている値に与えられた関数を適用する。`Option` が `None` の場合は与えられたデフォルト値を返す
* `map_or_else` は `Some` の含まれている値に与えられた関数を適用する。`Option` が `None` の場合は与えられたフォールバック関数を評価した結果を返す

これらのメソッドは2つの `Option` 値の `Some` バリアントを組み合わせます。

* `zip` は、`self` が `Some(s)` であり与えられた `Option` の値が `Some(o)` であれば `Some((s, o))` を返す。そうでなければ `None` を返す
* `zip_with` は、`self` が `Some(s)` であり与えられた `Option` の値が `Some(o)` であれば、与えられた関数 `f` を呼び出し `Some(f(s, o))` を返す。そうでなければ `None` を返す

### 真偽値演算子

これらのメソッドは `Option` を真偽値として扱い、`Some` は `true` のように、`None` は `false` のように振る舞います。これらのメソッドには2種類あります。1つは `Option` を入力に取るもの、もう1つは（遅延評価される）関数を入力に取るものです。

`and`、`or`、`xor` メソッドは別の `Option` を入力に取り、`Option` を出力として生成します。`and` メソッドだけが、`Option<T>` とは異なる内側の型 `U` を持つ `Option<U>` の値を生成できます。

| メソッド | self | 入力 | 出力 |
|--------|------|-------|--------|
| `and` | `None` | （無視される） | `None` |
| `and` | `Some(x)` | `None` | `None` |
| `and` | `Some(x)` | `Some(y)` | `Some(y)` |
| `or` | `None` | `None` | `None` |
| `or` | `None` | `Some(y)` | `Some(y)` |
| `or` | `Some(x)` | （無視される） | `Some(x)` |
| `xor` | `None` | `None` | `None` |
| `xor` | `None` | `Some(y)` | `Some(y)` |
| `xor` | `Some(x)` | `None` | `Some(x)` |
| `xor` | `Some(x)` | `Some(y)` | `None` |

`and_then` メソッドと `or_else` メソッドは関数を入力に取り、新しい値を生成する必要があるときにだけその関数を評価します。`and_then` メソッドだけが、`Option<T>` とは異なる内側の型 `U` を持つ `Option<U>` の値を生成できます。

| メソッド | self | 関数の入力 | 関数の結果 | 出力 |
|--------|------|----------------|-----------------|--------|
| `and_then` | `None` | （渡されない） | （評価されない） | `None` |
| `and_then` | `Some(x)` | `x` | `None` | `None` |
| `and_then` | `Some(x)` | `x` | `Some(y)` | `Some(y)` |
| `or_else` | `None` | （渡されない） | `None` | `None` |
| `or_else` | `None` | （渡されない） | `Some(y)` | `Some(y)` |
| `or_else` | `Some(x)` | （渡されない） | （評価されない） | `Some(x)` |

これは、`and_then` や `or` のようなメソッドをメソッド呼び出しのパイプラインで使う例です。パイプラインの前段では失敗の値 (`None`) はそのまま通過し、成功の値 (`Some`) に対する処理が続きます。パイプラインの終盤で、`None` を受け取った場合に `or` がエラーメッセージを代わりに入れます。

```rust
let mut bt = BTreeMap::new();
bt.insert(20u8, "foo");
bt.insert(42u8, "bar");
let res = [0u8, 1, 11, 200, 22]
    .into_iter()
    .map(|x| {
        // `checked_sub()` はエラー時に `None` を返す
        x.checked_sub(1)
            // `checked_mul()` も同様
            .and_then(|x| x.checked_mul(2))
            // `BTreeMap::get` はエラー時に `None` を返す
            .and_then(|x| bt.get(&x))
            // ここまでで `None` だった場合、エラーメッセージを代入する
            .or(Some(&"error!"))
            .copied()
            // 上で無条件に Some を使っているのでパニックしない
            .unwrap()
    })
    .collect::<Vec<_>>();
assert_eq!(res, ["error!", "error!", "foo", "error!", "bar"]);
```

### 比較演算子

`T` が `PartialOrd` を実装していれば、`Option<T>` はその `PartialOrd` 実装を導出します。この順序では、`None` はどの `Some` よりも小さいと比較され、2つの `Some` は、それらが含む値が `T` において比較される方法と同じように比較されます。`T` が `Ord` も実装していれば、`Option<T>` もそうなります。

```rust
assert!(None < Some(0));
assert!(Some(0) < Some(1));
```

### `Option` に対する反復

`Option` は反復できます。これは、条件によって空になるイテレータが必要な場合に役立ちます。このイテレータは（`Option` が `Some` のとき）単一の値を生成するか、（`Option` が `None` のとき）値を生成しません。たとえば、`into_iter` は `Option` が `Some(v)` のとき `once(v)` のように振る舞い、`None` のとき `empty()` のように振る舞います。

`Option<T>` に対するイテレータには3種類あります。

* `into_iter` は `Option` を消費し、含まれている値を生成する
* `iter` は含まれている値への不変参照 `&T` を生成する
* `iter_mut` は含まれている値への可変参照 `&mut T` を生成する

`Option` に対するイテレータは、イテレータを連結するとき、たとえば条件に応じて項目を挿入するときに便利です（イテレータのコンストラクタを明示的に呼ぶ必要が常にあるわけではありません。他のイテレータを受け取る多くの `Iterator` メソッドは、`IntoIterator` を実装する反復可能な型も受け付けます。`Option` もこれに含まれます）。

```rust
let yep = Some(42);
let nope = None;
// chain() は内部で into_iter() を呼ぶので、明示的に呼ぶ必要はない
let nums: Vec<i32> = (0..4).chain(yep).chain(4..8).collect();
assert_eq!(nums, [0, 1, 2, 3, 42, 4, 5, 6, 7]);
let nums: Vec<i32> = (0..4).chain(nope).chain(4..8).collect();
assert_eq!(nums, [0, 1, 2, 3, 4, 5, 6, 7]);
```

このようにイテレータを連結する理由の1つは、`impl Iterator` を返す関数は、すべての可能な戻り値が同じ具体的な型でなければならないからです。反復可能な `Option` を連結することは、それに役立ちます。

```rust
fn make_iter(do_insert: bool) -> impl Iterator<Item = i32> {
    // 戻り値の型が一致することを示すための明示的な return
    match do_insert {
        true => return (0..4).chain(Some(42)).chain(4..8),
        false => return (0..4).chain(None).chain(4..8),
    }
}
println!("{:?}", make_iter(true).collect::<Vec<_>>());
println!("{:?}", make_iter(false).collect::<Vec<_>>());
```

同じことを `once()` と `empty()` を使って行おうとすると、戻り値の具体的な型が異なってしまうため、`impl Iterator` を返せなくなります。

```rust
// 関数からのすべての可能な戻り値が同じ具体的な型でなければならないため、
// これはコンパイルできない。
fn make_iter(do_insert: bool) -> impl Iterator<Item = i32> {
    // 戻り値の型が一致しないことを示すための明示的な return
    match do_insert {
        true => return (0..4).chain(once(42)).chain(4..8),
        false => return (0..4).chain(empty()).chain(4..8),
    }
}
```

### `Option` への収集

`Option` は `FromIterator` トレイトを実装しています。これにより、`Option` 値に対するイテレータを、元の `Option` 値それぞれに含まれていた値のコレクションを持つ `Option` へ、あるいは要素のいずれかが `None` であれば `None` へ、収集できます。

```rust
let v = [Some(2), Some(4), None, Some(8)];
let res: Option<Vec<_>> = v.into_iter().collect();
assert_eq!(res, None);
let v = [Some(2), Some(4), Some(8)];
let res: Option<Vec<_>> = v.into_iter().collect();
assert_eq!(res, Some(vec![2, 4, 8]));
```

`Option` は `Product` トレイトと `Sum` トレイトも実装しているため、`Option` 値に対するイテレータは `product` メソッドと `sum` メソッドを使えます。

```rust
let v = [None, Some(1), Some(2), Some(3)];
let res: Option<i32> = v.into_iter().sum();
assert_eq!(res, None);
let v = [Some(1), Some(2), Some(21)];
let res: Option<i32> = v.into_iter().product();
assert_eq!(res, Some(42));
```

### `Option` をその場で変更する

これらのメソッドは、`Option<T>` に含まれている値への可変参照を返します。

* `insert` は値を挿入し、古い内容を破棄する
* `get_or_insert` は現在の値を取得する。`None` であれば与えられたデフォルト値を挿入する
* `get_or_insert_default` は現在の値を取得する。`None` であれば型 `T`（`Default` を実装していなければならない）のデフォルト値を挿入する
* `get_or_insert_with` は現在の値を取得する。`None` であれば与えられた関数で計算したデフォルト値を挿入する

これらのメソッドは `Option` に含まれている値の所有権を移します。

* `take` は `Option` に含まれている値（あれば）の所有権を取り、`Option` を `None` に置き換える
* `replace` は `Option` に含まれている値（あれば）の所有権を取り、`Option` を与えられた値を含む `Some` に置き換える

## 使用例

`Option` に対する基本的なパターンマッチング:

```rust
let msg = Some("howdy");

// 含まれている文字列への参照を取る
if let Some(m) = &msg {
    println!("{}", *m);
}

// 含まれている文字列を取り除き、Option を破棄する
let unwrapped_msg = msg.unwrap_or("default message");
```

ループの前に結果を `None` で初期化する:

```rust
enum Kingdom { Plant(u32, &'static str), Animal(u32, &'static str) }

// 検索対象となるデータの一覧。
let all_the_big_things = [
    Kingdom::Plant(250, "redwood"),
    Kingdom::Plant(230, "noble fir"),
    Kingdom::Plant(229, "sugar pine"),
    Kingdom::Animal(25, "blue whale"),
    Kingdom::Animal(19, "fin whale"),
    Kingdom::Animal(15, "north pacific right whale"),
];

// 最大の動物の名前を検索するが、最初はただの None から始める。
let mut name_of_biggest_animal = None;
let mut size_of_biggest_animal = 0;
for big_thing in &all_the_big_things {
    match *big_thing {
        Kingdom::Animal(size, name) if size > size_of_biggest_animal => {
            // ここで何か大きな動物の名前が見つかった
            size_of_biggest_animal = size;
            name_of_biggest_animal = Some(name);
        }
        Kingdom::Animal(..) | Kingdom::Plant(..) => ()
    }
}

match name_of_biggest_animal {
    Some(name) => println!("the biggest animal is {name}"),
    None => println!("there are no animals :("),
}
```

## 構造体

| 名前 | 説明 |
|------|-------------|
| [IntoIter](struct.IntoIter.html) | `Option` の `Some` バリアントの値に対するイテレータ。 |
| [Iter](struct.Iter.html) | `Option` の `Some` バリアントへの参照に対するイテレータ。 |
| [IterMut](struct.IterMut.html) | `Option` の `Some` バリアントへの可変参照に対するイテレータ。 |
| [OptionFlatten](struct.OptionFlatten.html) | `Option::into_flat_iter` が生成するイテレータ。詳細はそのドキュメントを参照。 |

## 列挙型

| 名前 | 説明 |
|------|-------------|
| [Option](enum.Option.html) | `Option` 型。詳細はモジュールレベルのドキュメントを参照。 |

---

本ページは [`std::option` (stable)](https://doc.rust-lang.org/stable/std/option/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
