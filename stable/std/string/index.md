---
title: string
---

# Module string

UTF-8 エンコードされた、サイズ可変な文字列。

このモジュールには [`String`](./struct.String.md) 型、文字列への変換のための [`ToString`](/stable/std/string/) トレイト、そして [`String`](./struct.String.md) を扱う際に生じうるいくつかのエラー型が含まれています。

## 使用例

文字列リテラルから新しい [`String`](./struct.String.md) を作る方法は複数あります。

```rust
let s = "Hello".to_string();

let s = String::from("world");
let s: String = "also this".into();
```

既存の [`String`](./struct.String.md) から、`+` で連結して新しい [`String`](./struct.String.md) を作ることもできます。

```rust
let s = "Hello".to_string();

let message = s + " world!";
```

正当な UTF-8 バイトのベクタがあれば、そこから [`String`](./struct.String.md) を作ることができます。逆方向の変換も可能です。

```rust
let sparkle_heart = vec![240, 159, 146, 150];

// これらのバイトが正当であることが分かっているので、`unwrap()` を使う。
let sparkle_heart = String::from_utf8(sparkle_heart).unwrap();

assert_eq!("💖", sparkle_heart);

let bytes = sparkle_heart.into_bytes();

assert_eq!(bytes, [240, 159, 146, 150]);
```

## 構造体

| 名前 | 説明 |
|------|-------------|
| [`Drain`](struct.Drain.html) | `String` のための、draining イテレータ。 |
| [`FromUtf8Error`](struct.FromUtf8Error.html) | UTF-8 バイトベクタから `String` へ変換する際に生じうるエラー値。 |
| [`FromUtf16Error`](struct.FromUtf16Error.html) | UTF-16 バイトスライスから `String` へ変換する際に生じうるエラー値。 |
| [`String`](./struct.String.md) | UTF-8 エンコードされた、サイズ可変な文字列。 |
| [`IntoChars`](struct.IntoChars.html) | 文字列の [`char`](/stable/std/primitive.char.html) に対するイテレータ。 |

## トレイト

| 名前 | 説明 |
|------|-------------|
| [`ToString`](trait.ToString.html) | 値を `String` へ変換するためのトレイト。 |

## 型エイリアス

| 名前 | 説明 |
|------|-------------|
| [`ParseError`](type.ParseError.html) | [`Infallible`](/stable/std/convert/) への型エイリアス。 |

---

本ページは [`std::string` (stable)](https://doc.rust-lang.org/stable/std/string/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
