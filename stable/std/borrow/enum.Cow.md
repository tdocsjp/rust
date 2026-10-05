---
title: Cow
---

# Enum Cow（要約）

```rust
pub enum Cow<'a, B: ?Sized + 'a + ToOwned> {
    Borrowed(&'a B),
    Owned(<B as ToOwned>::Owned),
}
```

`Cow<'a, B>`（clone-on-write）は、クローン・オン・ライトの機能を提供するスマートポインタです。借用データを効率的に扱いつつ、実際に変更が必要になるまでクローンのコストを遅延させます。

## バリアント

- **`Borrowed(&'a B)`** — 借用データへの参照を保持する
- **`Owned(<B as ToOwned>::Owned)`** — 所有データを保持する

## 主なメソッド

### `to_mut()` — 可変アクセスを取得する

所有形式への可変参照を返します。必要であればクローンします。

```rust
let mut cow = Cow::Borrowed("foo");
cow.to_mut().make_ascii_uppercase();
assert_eq!(cow, Cow::Owned(String::from("FOO")));
```

### `into_owned()` — 所有データを取り出す

所有データへ変換します。借用されていた場合はクローンします。

```rust
let cow = Cow::Borrowed("hello");
let owned: String = cow.into_owned(); // 文字列をクローンする
```

## 典型的な使い方

**文字列での使用:**
```rust
let input: &str = "data";
let mut cow = Cow::from(input);
// まだクローンされていない - 借用のまま
if needs_mutation {
    cow.to_mut().push_str(" modified"); // ここでのみクローンされる
}
```

**スライスでの使用:**
```rust
fn abs_all(input: &mut Cow<'_, [i32]>) {
    for i in 0..input.len() {
        if input[i] < 0 {
            input.to_mut()[i] = -input[i]; // 必要なら Vec へクローンする
        }
    }
}
```

## Cow を使う場面

- **最適化** — 変更が必要ないかもしれない場合に、不要な確保を避ける
- **汎用的な関数** — 借用データと所有データを統一的に受け取る
- **性能が重要なコード** — 実際に変更が必要になるまでクローンを遅延させる
- **構造体のフィールド** — 単一のライフタイムで、借用か所有かが柔軟なデータを保持する

## 主なトレイト実装

- `Deref` を実装しており、借用データへ透過的にアクセスできる
- `Clone`、`Debug`、`Display`、`Eq`、`PartialEq`、`Hash`、`Ord`、`PartialOrd`
- `From` によって `String`、`Vec`、`Path`、`OsString`、`CString` との間で変換できる

---

本ページは [`std::borrow::Cow` (stable)](https://doc.rust-lang.org/stable/std/borrow/enum.Cow.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/borrow/enum.Cow.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
