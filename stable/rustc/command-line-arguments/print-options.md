---
title: Print Options
---

# Print オプション

これらのオプションはすべて、`--print` フラグを通じて `rustc` に渡されます。

これらのオプションは、コンパイラに関する様々な情報を出力します。複数のオプションを指定でき、情報はオプションを指定した順に出力されます。

オプションを指定すると、通常は `--emit` のステップが無効化され、要求された情報だけが出力されます。

`--emit` と同様に、`--print KIND=PATH` の形式で、要求した各種類の情報ごとにファイルパスを任意で指定できます。パスを指定すると、情報は標準出力の代わりにそこへ書き込まれます。

## `crate-name`

クレートの名前。通常は `#![crate_name = "..."]` 属性、`--crate-name` フラグ、またはファイル名から取られます。

例:

```
$ rustc --print crate-name --crate-name my_crate a.rs
my_crate
```

## `file-names`

`link` emit 種別によって作成されるファイルの名前。

## `sysroot`

sysroot への絶対パス。

例（rustup と stable ツールチェインの場合):

```
$ rustc --print sysroot a.rs
/home/[REDACTED]/.rustup/toolchains/stable-x86_64-unknown-linux-gnu
```

## `target-libdir`

ターゲットの libdir へのパス。

例（rustup と stable ツールチェインの場合):

```
$ rustc --print target-libdir a.rs
/home/[REDACTED]/.rustup/toolchains/beta-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib
```

## `host-tuple`

ホストコンパイラのターゲットタプル文字列。

例:

```
$ rustc --print host-tuple a.rs
x86_64-unknown-linux-gnu
```

`--target` フラグとの例:

```
$ rustc --print host-tuple --target "armv7-unknown-linux-gnueabihf" a.rs
x86_64-unknown-linux-gnu
```

## `cfg`

cfg 値の一覧。cfg 値についての詳細は条件付きコンパイルを参照してください。

例（`x86_64-unknown-linux-gnu` の場合):

```
$ rustc --print cfg a.rs
debug_assertions
panic="unwind"
target_abi=""
target_arch="x86_64"
target_endian="little"
target_env="gnu"
target_family="unix"
target_feature="fxsr"
target_feature="sse"
target_feature="sse2"
target_has_atomic="16"
target_has_atomic="32"
target_has_atomic="64"
target_has_atomic="8"
target_has_atomic="ptr"
target_os="linux"
target_pointer_width="64"
target_vendor="unknown"
unix
```

## `target-list`

既知のターゲットの一覧。`--target` フラグでターゲットを選べます。

## `target-cpus`

現在のターゲットで使える CPU の値の一覧。`-C target-cpu=val` フラグでターゲット CPU を選べます。

## `target-features`

_現在のターゲット_で使えるターゲットフィーチャーの一覧。

ターゲットフィーチャーは **unsafe** な `-C target-feature=val` フラグで有効化できます。

詳細は既知の問題を参照してください。

## `relocation-models`

リロケーションモデルの一覧。`-C relocation-model=val` フラグでリロケーションモデルを選べます。

例:

```
$ rustc --print relocation-models a.rs
Available relocation models:
    static
    pic
    pie
    dynamic-no-pic
    ropi
    rwpi
    ropi-rwpi
    default
```

## `code-models`

コードモデルの一覧。`-C code-model=val` フラグでコードモデルを選べます。

例:

```
$ rustc --print code-models a.rs
Available code models:
    tiny
    small
    kernel
    medium
    large
```

## `tls-models`

サポートされているスレッドローカルストレージ (TLS) モデルの一覧。`-Z tls-model=val` フラグでモデルを選べます。

例:

```
$ rustc --print tls-models a.rs
Available TLS models:
    global-dynamic
    local-dynamic
    initial-exec
    local-exec
    emulated
```

## `native-static-libs`

`staticlib` クレート種別を作る際に使えます。

これが唯一のフラグである場合、完全なコンパイルが行われ、生成された静的ライブラリをリンクする際に使うべきリンカフラグを示す診断ノートが含まれます。

このノートは `native-static-libs:` というテキストで始まり、出力を取得しやすくしています。

例:

```
$ rustc --print native-static-libs --crate-type staticlib a.rs
note: link against the following native artifacts when linking against this static library. The order and any duplication can be significant on some platforms.

note: native-static-libs: -lgcc_s -lutil [REDACTED] -lpthread -lm -ldl -lc
```

## `link-args`

このフラグは `--emit` ステップを無効化しません。リンカオプションのデバッグに便利です。

リンク時、このフラグは `rustc` に、完全なリンカの呼び出しを人間が読める形式で出力させます。このデバッグ出力の正確な形式は安定した保証ではありませんが、リンカの実行ファイルと、リンカに渡される各コマンドライン引数のテキストが含まれます。

## `deployment-target`

選択された Apple プラットフォームターゲットに対する、現在選択されているデプロイメントターゲット（または最小 OS バージョン）。

この値は、この情報を必要とする C コンパイラのような、Rust ビルドに付随する他のコンポーネントに使ったり渡したりできます。環境に `*_DEPLOYMENT_TARGET` 変数が存在しなければ、rustc がサポートする最小のデプロイメントターゲットを返し、そうでなければその変数を解析した値を返します。

---

本ページは [Print Options (stable)](https://doc.rust-lang.org/stable/rustc/command-line-arguments/print-options.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
