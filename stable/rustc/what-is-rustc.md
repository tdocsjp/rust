---
title: rustc とは何か
---

# rustc とは何か

「The rustc book」へようこそ！ `rustc` は、プロジェクト自身が提供する Rust 言語のコンパイラです。コンパイラは、ソースコードを受け取り、ライブラリまたは実行可能ファイルとしてバイナリコードを生成します。

ほとんどの Rust プログラマは `rustc` を直接呼び出さず、[Cargo](/stable/cargo/) を通じて呼び出します。とはいえ、すべては `rustc` に奉仕するためのものです！ Cargo が `rustc` をどう呼んでいるか見たいなら、次のようにできます。

```
$ cargo build --verbose
```

これで、各 `rustc` の呼び出しが出力されます。この本は、これらの各オプションが何をするのかを理解する助けになります。さらに、ほとんどの Rustacean は Cargo を使いますが、全員がそうではありません。`rustc` を他のビルドシステムに統合することもあります。この本は、そのために必要なすべてのオプションのガイドを提供するはずです。

## 基本的な使い方

`hello.rs` というファイルに、小さな Hello World プログラムがあるとしましょう。

```rust
fn main() {
    println!("Hello, world!");
}
```

このソースコードを実行可能ファイルにするには、`rustc` を使います。

```
$ rustc hello.rs
$ ./hello # *NIX の場合
$ .\hello.exe # Windows の場合
```

ここで、`rustc` にはコンパイルしたいすべてのファイルではなく、_クレートルート_だけを渡すことに注意してください。たとえば、次のような `main.rs` があるとします。

```rust
mod foo;

fn main() {
    foo::hello();
}
```

そして、次のような `foo.rs` があるとします。

```rust
pub fn hello() {
    println!("Hello, world!");
}
```

これをコンパイルするには、次のコマンドを実行します。

```
$ rustc main.rs
```

`foo.rs` について `rustc` に伝える必要はありません。`mod` 文が必要なものをすべて与えてくれます。これは C コンパイラの使い方とは異なります。C ではファイルごとにコンパイラを呼び出し、その後すべてをリンクします。言い換えると、_クレート_は特定のモジュールではなく、翻訳単位 (translation unit) なのです。

---

本ページは [What is rustc? (stable)](https://doc.rust-lang.org/stable/rustc/what-is-rustc.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
