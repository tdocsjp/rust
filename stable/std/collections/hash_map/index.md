---
title: hash_map
---

# Module hash_map

二次探査 (quadratic probing) と SIMD ルックアップで実装されたハッシュマップ。

## 構造体

| 名前 | 説明 |
|------|-------------|
| [DefaultHasher](struct.DefaultHasher.html) | [RandomState](struct.RandomState.html) が使う既定の [Hasher](/stable/std/hash/)。 |
| [Drain](struct.Drain.html) | `HashMap` のエントリに対する draining イテレータ。 |
| [ExtractIf](struct.ExtractIf.html) | `HashMap` のエントリに対する、絞り込みを行う draining イテレータ。 |
| [HashMap](../struct.HashMap.md) | 二次探査と SIMD ルックアップで実装された[ハッシュマップ](/stable/std/collections/)。 |
| [IntoIter](struct.IntoIter.html) | `HashMap` のエントリを所有するイテレータ。 |
| [IntoKeys](struct.IntoKeys.html) | `HashMap` のキーを所有するイテレータ。 |
| [IntoValues](struct.IntoValues.html) | `HashMap` の値を所有するイテレータ。 |
| [Iter](struct.Iter.html) | `HashMap` のエントリに対するイテレータ。 |
| [IterMut](struct.IterMut.html) | `HashMap` のエントリに対する可変イテレータ。 |
| [Keys](struct.Keys.html) | `HashMap` のキーに対するイテレータ。 |
| [OccupiedEntry](struct.OccupiedEntry.html) | `HashMap` の、値が入っているエントリを表すビュー。[Entry](enum.Entry.html) enum の一部。 |
| [RandomState](struct.RandomState.html) | `RandomState` は [HashMap](../struct.HashMap.md) 型の既定の state。 |
| [VacantEntry](struct.VacantEntry.html) | `HashMap` の、空いているエントリを表すビュー。[Entry](enum.Entry.html) enum の一部。 |
| [Values](struct.Values.html) | `HashMap` の値に対するイテレータ。 |
| [ValuesMut](struct.ValuesMut.html) | `HashMap` の値に対する可変イテレータ。 |
| [OccupiedError](struct.OccupiedError.html)（実験的） | キーが既に存在するときに [try_insert](../struct.HashMap.md) が返すエラー。 |

## 列挙型

| 名前 | 説明 |
|------|-------------|
| [Entry](enum.Entry.html) | マップの単一エントリを表すビュー。空いているか、値が入っているかのいずれか。 |

---

本ページは [`std::collections::hash_map` (stable)](https://doc.rust-lang.org/stable/std/collections/hash_map/index.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
