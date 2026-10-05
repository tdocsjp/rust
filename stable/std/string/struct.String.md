---
title: String
---

# Struct String

```rust
pub struct String { /* private fields */ }
```

UTF-8 エンコードされた、サイズ可変な文字列。

`String` は最も一般的な文字列型です。文字列の内容に対する所有権を持ち、その内容はヒープに確保されたバッファに格納されます（[表現](#表現representation)を参照）。これは、借用版に相当するプリミティブ型 [`str`](/stable/std/primitive.str.html) と密接に関連しています。

## 使用例

[`String::from`](/stable/std/convert/) を使って、[文字列リテラル](/stable/std/primitive.str.html)から `String` を作成できます。

```rust
let hello = String::from("Hello, world!");
```

[`push`](#methods) メソッドで `String` に [`char`](/stable/std/primitive.char.html) を追加したり、[`push_str`](#methods) メソッドで [`&str`](/stable/std/primitive.str.html) を追加したりできます。

```rust
let mut hello = String::from("Hello, ");

hello.push('w');
hello.push_str("orld!");
```

UTF-8 バイトのベクタがあれば、[`from_utf8`](#associated-functions) メソッドでそれから `String` を作成できます。

```rust
// ベクタの中の何らかのバイト列
let sparkle_heart = vec![240, 159, 146, 150];

// これらのバイトが正当であることが分かっているので、`unwrap()` を使う。
let sparkle_heart = String::from_utf8(sparkle_heart).unwrap();

assert_eq!("💖", sparkle_heart);
```

## UTF-8

`String` は常に正当な UTF-8 です。UTF-8 でない文字列が必要な場合は、[`OsString`](/stable/std/ffi/) を検討してください。似ていますが、UTF-8 の制約がありません。UTF-8 は可変幅のエンコーディングなので、`String` は通常、同じ `char` の配列よりも小さくなります。

```rust
// `s` は ASCII で、各 `char` を1バイトとして表す
let s = "hello";
assert_eq!(s.len(), 5);

// 同じ内容の `char` 配列は、各 `char` が4バイトになるため長くなる
let s = ['h', 'e', 'l', 'l', 'o'];
let size: usize = s.into_iter().map(|c| size_of_val(&c)).sum();
assert_eq!(size, 20);

// しかし非 ASCII の文字列では、差はより小さくなり、
// 同じになることもある
let s = "💖💖💖💖💖";
assert_eq!(s.len(), 20);

let s = ['💖', '💖', '💖', '💖', '💖'];
let size: usize = s.into_iter().map(|c| size_of_val(&c)).sum();
assert_eq!(size, 20);
```

これは `s[i]` がどう動作すべきかという興味深い問いを提起します。ここで `i` は何であるべきでしょうか。バイトインデックスや `char` インデックスなどいくつかの選択肢がありますが、UTF-8 エンコーディングのため、バイトインデックスだけが定数時間のインデックスアクセスを提供できます。たとえば `i` 番目の `char` を取得するには、[`chars`](/stable/std/primitive.str.html) を使います。

```rust
let s = "hello";
let third_character = s.chars().nth(2);
assert_eq!(third_character, Some('l'));

let s = "💖💖💖💖💖";
let third_character = s.chars().nth(2);
assert_eq!(third_character, Some('💖'));
```

次に、`s[i]` は何を返すべきでしょうか。インデックスアクセスは基礎データへの参照を返すので、`&u8`、`&[u8]`、あるいは類似の何かになり得ます。1つのインデックスしか与えていないので `&u8` が最も意味が通りますが、それはユーザーが期待するものではないかもしれません。これは [`as_bytes()`](/stable/std/primitive.str.html) で明示的に実現できます。

```rust
// 最初のバイトは104 - `'h'` のバイト値
let s = "hello";
assert_eq!(s.as_bytes()[0], 104);
// または
assert_eq!(s.as_bytes()[0], b'h');

// 最初のバイトは240で、明らかに有用ではない
let s = "💖💖💖💖💖";
assert_eq!(s.as_bytes()[0], 240);
```

これらの曖昧さ・制約のため、`usize` によるインデックスアクセスは単純に禁止されています。

```rust
let s = "hello";

// 次のコードはコンパイルできない！
println!("The first letter of s is {}", s[0]);
```

一方、`&s[i..j]`（つまり範囲によるインデックスアクセス）がどう動作すべきかはより明確です。これはバイトインデックスを受け取り（定数時間であるため）、UTF-8 エンコードされた `&str` を返すべきです。これは「文字列スライス化」とも呼ばれます。与えられたバイトインデックスが文字境界でない場合はパニックすることに注意してください。詳しくは [`is_char_boundary`](/stable/std/primitive.str.html) を参照してください。文字列スライス化の詳細については [`SliceIndex<str>`](/stable/std/slice/) の実装を参照してください。パニックしない版の文字列スライス化については [`get`](/stable/std/primitive.str.html) を参照してください。

[`bytes`](/stable/std/primitive.str.html) メソッドと [`chars`](/stable/std/primitive.str.html) メソッドは、それぞれ文字列のバイトとコードポイントに対するイテレータを返します。バイトインデックスとともにコードポイントを反復するには、[`char_indices`](/stable/std/primitive.str.html) を使います。

## Deref

`String` は `Deref<Target = str>` を実装しているため、[`str`](/stable/std/primitive.str.html) のすべてのメソッドを継承します。さらに、これは、アンパサンド (`&`) を使うことで、`String` を [`&str`](/stable/std/primitive.str.html) を受け取る関数に渡せることを意味します。

```rust
fn takes_str(s: &str) { }

let s = String::from("Hello");

takes_str(&s);
```

これは `String` から [`&str`](/stable/std/primitive.str.html) を作成して渡します。この変換は非常に低コストなので、特定の理由で `String` が必要でない限り、一般に関数は引数として [`&str`](/stable/std/primitive.str.html) を受け取るようにします。

場合によっては、Rust にはこの変換（[`Deref`](/stable/std/ops/) 型強制として知られる）を行うための十分な情報がありません。次の例では、文字列スライス `&'a str` がトレイト `TraitExample` を実装し、関数 `example_func` はそのトレイトを実装する任意の型を受け取ります。この場合、Rust は2段階の暗黙の変換を行う必要がありますが、Rust にはそれを行う手段がありません。そのため、次の例はコンパイルできません。

```rust
trait TraitExample {}

impl<'a> TraitExample for &'a str {}

fn example_func<A: TraitExample>(example_arg: A) {}

let example_string = String::from("example_string");
example_func(&example_string);
```

代わりに機能する選択肢が2つあります。1つ目は、`example_func(&example_string);` の行を `example_func(example_string.as_str());` に変更し、[`as_str()`](#methods) メソッドを使って、文字列を含む文字列スライスを明示的に取り出す方法です。2つ目は `example_func(&example_string);` を `example_func(&*example_string);` に変更する方法です。この場合、`String` を [`str`](/stable/std/primitive.str.html) へデリファレンスし、その [`str`](/stable/std/primitive.str.html) を再び [`&str`](/stable/std/primitive.str.html) へ参照しています。2つ目の方法がより慣用的ですが、どちらも暗黙の変換に頼らず明示的に変換を行います。

## 表現（Representation）

`String` は3つの要素からできています。バイト列へのポインタ、長さ、容量です。ポインタは、`String` がデータを格納するために使う内部バッファを指します。長さはそのバッファに現在格納されているバイト数であり、容量はそのバッファのバイト単位でのサイズです。したがって、長さは常に容量以下になります。

このバッファは常にヒープに格納されます。

これらは [`as_ptr`](/stable/std/primitive.str.html)、[`len`](#methods)、[`capacity`](#methods) メソッドで確認できます。

```rust
let story = String::from("Once upon a time...");

// String を各部分に分解する。
let (ptr, len, capacity) = story.into_raw_parts();

// story は19バイト
assert_eq!(19, len);

// ptr、len、capacity から String を再構築できる。
// 各要素が正当であることを保証する責任があるため、これはすべて unsafe:
let s = unsafe { String::from_raw_parts(ptr, len, capacity) } ;

assert_eq!(String::from("Once upon a time..."), s);
```

`String` が十分な容量を持っていれば、要素を追加しても再確保は発生しません。たとえば、次のプログラムを考えてみましょう。

```rust
let mut s = String::new();

println!("{}", s.capacity());

for _ in 0..5 {
    s.push_str("hello");
    println!("{}", s.capacity());
}
```

これは次のように出力します。

```
0
8
16
16
32
32
```

最初はメモリが全く確保されていませんが、文字列に追加していくにつれて、容量が適切に増えていきます。代わりに [`with_capacity`](#associated-functions) メソッドを使って、最初から正しい容量を確保すると:

```rust
let mut s = String::with_capacity(25);

println!("{}", s.capacity());

for _ in 0..5 {
    s.push_str("hello");
    println!("{}", s.capacity());
}
```

出力は次のように変わります。

```
25
25
25
25
25
25
```

ここでは、ループの中で追加のメモリを確保する必要がありません。

---

各メソッド（`push`・`push_str`・`as_str`・`from_utf8`・`with_capacity` など）の個別の説明文と例は今後追加予定です。最新の個別ページは [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/string/struct.String.html) を参照してください。

---

本ページは [`std::string::String` (stable)](https://doc.rust-lang.org/stable/std/string/struct.String.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
