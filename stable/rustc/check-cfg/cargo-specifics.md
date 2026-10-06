---
title: Cargo 特有の事情
---

# Cargo 特有の事情（要約）

Cargo は、`--check-cfg` フラグと `unexpected_cfgs` lint と、主に3つの仕組みで統合しています。

## 1. Cargo のフィーチャー

Cargo は `[features]` テーブルで定義されたすべてのフィーチャーについて、自動的に cfg を宣言します。

```toml
[features]
serde = ["dep:serde"]
my_feature = []
```

これらは自動的に、期待される cfg として認識されます。

## 2. `[lints.rust]` テーブル

静的に分かっているカスタムな設定については、`check-cfg` の lint 設定を使います。

```toml
[lints.rust]
unexpected_cfgs = { level = "warn", check-cfg = ['cfg(has_foo)'] }
```

これにより、静的に分かっているカスタムな cfg を事前に宣言できます。

## 3. ビルドスクリプト (`build.rs`)

`cargo::rustc-cfg` で動的に設定を行う場合、`cargo::rustc-check-cfg` でそれを宣言します。

```rust
fn main() {
    println!("cargo::rustc-check-cfg=cfg(has_foo)");
    if has_foo() {
        println!("cargo::rustc-cfg=has_foo");
    }
}
```

これにより、動的に生成された設定について `unexpected_cfgs` lint の警告が出ることを防げます（Cargo 1.80 以降）。

これら3つの方法によって、Cargo は予期しない cfg の警告を発生させずに、条件付きコンパイルを適切に検証できます。

---

本ページは [Cargo Specifics (stable)](https://doc.rust-lang.org/stable/rustc/check-cfg/cargo-specifics.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
