---
title: HashMap
---

# Struct HashMap

```rust
pub struct HashMap<K, V, S = RandomState, A: Allocator = Global> { /* private fields */ }
```

二次探査 (quadratic probing) と SIMD ルックアップで実装された[ハッシュマップ](/stable/std/collections/)。

## ハッシュと HashDoS 耐性

`HashMap` はデフォルトで、HashDoS 攻撃への耐性を持つように選ばれたハッシュアルゴリズムを使います。このアルゴリズムはランダムにシードされ、ホストが提供する高品質で安全な乱数源から、プログラムをブロックせずにこのシードを生成するよう、合理的な最善努力が行われます。そのため、シードのランダム性は、シード生成時のシステムの乱数生成器の出力品質に依存します。特に、システム起動時のようにエントロピープールが異常に少ないときに生成されたシードは、品質が低くなる場合があります。

デフォルトのハッシュアルゴリズムは現在 SipHash 1-3 ですが、これは将来いつでも変わる可能性があります。中くらいのサイズのキーに対する性能は非常に競争力がありますが、整数のような小さいキーや、長い文字列のような大きいキーに対しては、他のハッシュアルゴリズムの方が優れています。ただし、それらのアルゴリズムは通常 HashDoS のような攻撃から保護してくれ_ません_。

ハッシュアルゴリズムは `HashMap` ごとに、[`default`](/stable/std/default/)、[`with_hasher`](#methods)、[`with_capacity_and_hasher`](#associated-functions) メソッドを使って置き換えることができます。[crates.io には多数の代替ハッシュアルゴリズム](https://crates.io/keywords/hasher)があります。

## キーに対する要件

キーは [`Eq`](/stable/std/cmp/) トレイトと [`Hash`](/stable/std/hash/) トレイトを実装している必要がありますが、これは多くの場合 `#[derive(PartialEq, Eq, Hash)]` を使うことで達成できます。これらを自分で実装する場合、次の性質が成り立つことが重要です。

```
k1 == k2 -> hash(k1) == hash(k2)
```

つまり、2つのキーが等しければ、そのハッシュも等しくなければなりません。この性質に違反することは論理エラーです。

また、キーがマップに入っている間に、[`Hash`](/stable/std/hash/) トレイトが決定するハッシュや、[`Eq`](/stable/std/cmp/) トレイトが決定する等価性が変わるような形でキーが変更されることも論理エラーです。これは通常、[`Cell`](/stable/std/cell/)、[`RefCell`](/stable/std/cell/)、グローバル状態、I/O、unsafe なコードを通じてのみ可能です。

どちらの論理エラーから生じる挙動も規定されていませんが、その論理エラーを観測した `HashMap` の中にカプセル化され、未定義動作には至りません。パニック、誤った結果、abort、メモリリーク、非停止などが起こりうります。

## 実装の詳細

このハッシュテーブルの実装は、Google の [SwissTable](https://abseil.io/blog/20180927-swisstables) の Rust ポートです。元の C++ 版の SwissTable は[こちら](https://github.com/abseil/abseil-cpp/blob/master/absl/container/internal/raw_hash_set.h)で見られ、この[CppCon の講演](https://www.youtube.com/watch?v=ncHmEUmJZf4)がアルゴリズムの仕組みを概説しています。

## 使用例

型推論により、明示的な型注釈（この例では `HashMap<String, String>`）を省略できます。

```rust
use std::collections::HashMap;

let mut book_reviews = HashMap::new();

book_reviews.insert(
    "Adventures of Huckleberry Finn".to_string(),
    "My favorite book.".to_string(),
);

// コレクションが所有された値 (String) を格納していても、
// 参照 (&str) で問い合わせられる。
if !book_reviews.contains_key("Les Misérables") {
    println!("We've got {} reviews, but Les Misérables ain't one.",
             book_reviews.len());
}

book_reviews.remove("Adventures of Huckleberry Finn");
```

既知の項目一覧があれば、配列から `HashMap` を初期化できます。

```rust
use std::collections::HashMap;

let solar_distance = HashMap::from([
    ("Mercury", 0.4),
    ("Venus", 0.7),
    ("Earth", 1.0),
    ("Mars", 1.5),
]);
```

## Entry API

`HashMap` は [`Entry` API](#methods) を実装しており、キーとその値を取得・設定・更新・削除する複雑な操作を行えます。

```rust
use std::collections::HashMap;

let mut player_stats = HashMap::new();

// キーがまだ存在しない場合にのみ挿入する
player_stats.entry("health").or_insert(100);

// 既存の値を更新する（キーが未設定である可能性を考慮しつつ）
let stat = player_stats.entry("attack").or_insert(100);
*stat += 10;

// 挿入の前にその場でエントリを変更する
player_stats.entry("mana").and_modify(|mana| *mana += 200).or_insert(100);
```

## カスタムキー型での使用

カスタムキー型で `HashMap` を使う最も簡単な方法は、[`Eq`](/stable/std/cmp/) と [`Hash`](/stable/std/hash/) を derive することです（[`PartialEq`](/stable/std/cmp/) も derive する必要があります）。

```rust
use std::collections::HashMap;

#[derive(Hash, Eq, PartialEq, Debug)]
struct Viking {
    name: String,
    country: String,
}

let vikings = HashMap::from([
    (Viking { name: "Einar".into(), country: "Norway".into() }, 25),
]);
```

## `const` / `static` での使用

上述の通り `HashMap` はランダムにシードされるため、`HashMap::new` は通常 `const` や `static` の初期化子では使えません。ランダムなシード生成を保ったまま `const`/`static` で使いたい場合は、`HashMap` を [`LazyLock`](/stable/std/sync/) でラップしてください。あるいは、ランダムシードに依存しない別のハッシャーを使って `const`/`static` 初期化子の中で `HashMap` を構築することもできますが、**その方法で作られた `HashMap` は HashDoS 攻撃に対して耐性がないことに注意してください！**

---

各メソッド（`insert`・`get`・`remove`・`entry` など）の個別の説明文と例は今後追加予定です。最新の個別ページは [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/collections/struct.HashMap.html) を参照してください。

---

本ページは [`std::collections::HashMap` (stable)](https://doc.rust-lang.org/stable/std/collections/struct.HashMap.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
