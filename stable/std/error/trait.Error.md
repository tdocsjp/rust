---
title: Error
---

# Trait Error（要約）

```rust
pub trait Error: Debug + Display {
    fn source(&self) -> Option<&(dyn Error + 'static)> { ... }
    // その他、提供されるメソッド
}
```

`Error` トレイトは、`Result<T, E>` のエラー値に対する基本的な要件を表します。Rust でカスタムのエラー型を定義する標準的な方法です。

## 必須の要件

`Error` は次の2つのトレイトの実装を要求します。

- **`Debug`** — 詳細なデバッグ情報のため
- **`Display`** — ユーザー向けの分かりやすいエラーメッセージのため（通常は小文字で始まり、簡潔で、末尾に句読点を付けない）

```rust
let err = "NaN".parse::<u32>().unwrap_err();
assert_eq!(err.to_string(), "invalid digit found in string");
```

## 主なメソッド

### `source()` — エラーの連鎖

`Option<&(dyn Error + 'static)>` を返し、根本の原因にアクセスできるようにします。これにより、抽象化の境界を越えたエラーの連鎖が可能になり、上位のモジュールは自前のエラーを提供しつつ、デバッグのために実装の詳細を明らかにできます。

**重要な規則**: 根本のエラーは `source()` によって返される_か_、`Display` によって表示される_か_のいずれかであるべきで、両方ではありません。

## 典型的な実装パターン

```rust
use std::error::Error;
use std::fmt;
use std::path::PathBuf;

#[derive(Debug)]
struct ReadConfigError {
    path: PathBuf
}

impl fmt::Display for ReadConfigError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "unable to read configuration at {}", self.path.display())
    }
}

impl Error for ReadConfigError {}
```

最小限の `Error` 実装は `Debug` + `Display` だけを要求します。他のメソッドにはデフォルト実装があります。

## ダウンキャストメソッド

`dyn Error` トレイトオブジェクトに対しては、`downcast_ref<T>()`、`downcast_mut<T>()`、`is<T>()` のようなメソッドによって、実行時の型チェックと具体的なエラー型の復元ができます。

---

本ページは [`std::error::Error` (stable)](https://doc.rust-lang.org/stable/std/error/trait.Error.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/error/trait.Error.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
