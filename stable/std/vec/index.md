---
title: vec
---

# Module vec

ヒープ確保された内容を持つ、連続した可変長の配列型。`Vec<T>` と表記されます。

ベクタは _O_(1) のインデックスアクセス、（末尾への）償却 _O_(1) の push、（末尾からの）_O_(1) の pop を持ちます。

ベクタは、`isize::MAX` バイトを超えて確保することがないよう保証されています。

## 使用例

[`Vec::new`](./struct.Vec.md) を使って明示的に [`Vec`](./struct.Vec.md) を作成できます。

```rust
let v: Vec<i32> = Vec::new();
```

…あるいは [`vec!`](/stable/std/) マクロを使うこともできます。

```rust
let v: Vec<i32> = vec![];

let v = vec![1, 2, 3, 4, 5];

let v = vec![0; 10]; // 10個のゼロ
```

ベクタの末尾に値を [`push`](./struct.Vec.md) できます（必要に応じてベクタが大きくなります）。

```rust
let mut v = vec![1, 2];

v.push(3);
```

値を取り出す (pop) のもほぼ同じように動作します。

```rust
let mut v = vec![1, 2];

let two = v.pop();
```

ベクタは（[`Index`](/stable/std/ops/) トレイトと [`IndexMut`](/stable/std/ops/) トレイトを通じた）インデックスアクセスもサポートしています。

```rust
let mut v = vec![1, 2, 3];
let three = v[2];
v[1] = v[1] + 5;
```

## メモリレイアウト

型がゼロサイズでなく、容量 (capacity) がゼロでない場合、[`Vec`](./struct.Vec.md) はその確保のために [`Global`](/stable/std/alloc/) アロケータを使用します。そのような [`Vec`](./struct.Vec.md) と、[`Global`](/stable/std/alloc/) アロケータで確保された生ポインタとの間を双方向に変換することは、アロケータに使われる [`Layout`](/stable/std/alloc/) が、その型の `capacity` 個の要素からなるシーケンスに対して正しいものであり、生ポインタが指す最初の `len` 個の値が有効である限り、妥当です。より正確には、[`Layout::array::<T>(capacity)`](/stable/std/alloc/) を用いて [`Global`](/stable/std/alloc/) アロケータで確保された `ptr: *mut T` は、[`Vec::<T>::from_raw_parts(ptr, len, capacity)`](./struct.Vec.md) を使って vec へ変換できます。逆に、[`Vec::<T>::as_mut_ptr`](./struct.Vec.md) から得られる `value: *mut T` の背後にあるメモリは、同じレイアウトを使って [`Global`](/stable/std/alloc/) アロケータで解放できます。

ゼロサイズ型 (ZST) の場合、あるいは容量がゼロの場合、`Vec` のポインタは非 null で十分にアラインされている必要があります。[`vec!`](/stable/std/) が使えない場合に ZST の `Vec` を構築する推奨される方法は、[`ptr::NonNull::dangling`](/stable/std/ptr/) を使うことです。

## 構造体

| 名前 | 説明 |
|------|-------------|
| [Drain](struct.Drain.html) | `Vec<T>` のための、draining イテレータ。 |
| [ExtractIf](struct.ExtractIf.html) | クロージャを使って要素を削除すべきかどうかを判定するイテレータ。 |
| [IntoIter](struct.IntoIter.html) | ベクタから要素を move で取り出すイテレータ。 |
| [Splice](struct.Splice.html) | `Vec` のための、splice（差し替え）を行うイテレータ。 |
| [Vec](./struct.Vec.md) | `Vec<T>` と表記される、連続した可変長の配列型。「vector」の略。 |
| [PeekMut](struct.PeekMut.html) | `Vec` の最後の要素への可変参照をラップする構造体。 |

---

本ページは [`std::vec` (stable)](https://doc.rust-lang.org/stable/std/vec/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
