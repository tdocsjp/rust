---
title: Path / PathBuf
---

# Struct Path・PathBuf（要約）

```rust
pub struct Path { /* private fields */ }
pub struct PathBuf { /* private fields */ }
```

`Path` と `PathBuf` は、Unix 系（`/` 区切り）と Windows（`\` または `/` 区切り）の違いを吸収する、クロスプラットフォームなファイルパスの抽象です。

`Path` はサイズ不定の借用スライス型（`str` のようなもの）、`PathBuf` は所有権を持つヒープ確保型（`String` のようなもの）です。`str`/`String` の関係とちょうど対応しています。

## 特徴

- **クロスプラットフォーム** — OS 固有のパス区切り文字や規約を吸収する
- **サイズ不定の型** — `Path` は常にポインタの背後（`&Path`、`Box<Path>` など）で使われる
- **Unicode の扱い** — 内部的に `OsStr` を使い、UTF-8・非 UTF-8 の両方のパスに対応する

## 主なメソッド

**構築・変換:**
```rust
Path::new("foo.txt")           // str スライスから作る
path.as_os_str()               // 内部の OsStr を取得する
path.to_str()                  // &str へ変換する（正当な UTF-8 の場合）
path.to_string_lossy()         // 不正な UTF-8 を置き換えつつ String へ変換する
path.to_path_buf()             // 所有権を持つ PathBuf へ変換する
```

**パスの探索:**
```rust
path.parent()                  // 親ディレクトリを取得する
path.file_name()               // 最後の要素を取得する（例: "file.txt"）
path.file_stem()               // 拡張子を除いた名前を取得する（"file.txt" から "file"）
path.extension()               // ドットを除いた拡張子を取得する（"txt"）
```

**パスの操作:**
```rust
path.join("subdir")            // パス要素を追加する
path.with_file_name("new.rs")  // 最後の要素を置き換える
path.with_extension("md")      // 拡張子を置き換える
path.strip_prefix("/etc")      // 接頭辞を取り除く（Result を返す）
path.starts_with("/etc")       // 指定した接頭辞で始まるか確認する
```

**ファイルシステム操作:**
```rust
path.exists()                  // パスが存在するか確認する（エラー時は false）
path.is_file()                 // 通常のファイルか確認する
path.is_dir()                  // ディレクトリか確認する
path.canonicalize()            // シンボリックリンクを解決した絶対パスを取得する
path.read_dir()                // ディレクトリの内容を反復する
```

## 使用例

```rust
use std::path::Path;

let path = Path::new("/tmp/foo.txt");

let parent = path.parent();           // Some("/tmp")
let file_name = path.file_name();     // Some("foo.txt")
let ext = path.extension();           // Some("txt")

let new_path = path.with_extension("rs");  // /tmp/foo.rs

if path.exists() {
    println!("File exists: {}", path.display());
}
```

## Path と PathBuf

| 特徴 | `Path` | `PathBuf` |
|---------|--------|----------|
| 種類 | サイズ不定のスライス | 所有権を持つヒープ確保 |
| 対応関係 | `str` | `String` |
| 用途 | 借用・参照 | 所有・変更 |

`Path` に対するほとんどの操作は、その場で変更するのではなく新しい `PathBuf` を返し、不変のセマンティクスを保ちます。

---

本ページは [`std::path::Path` / `PathBuf` (stable)](https://doc.rust-lang.org/stable/std/path/struct.Path.html) の要約の非公式日本語訳です。各メソッドの個別の説明文と例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/std/path/struct.Path.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
