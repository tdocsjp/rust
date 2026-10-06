---
title: Jobserver
---

# Jobserver

内部的に、`rustc` は並列性を活用することがあります。`rustc` は、自身を呼び出すビルドシステムが `MAKEFLAGS` 環境変数で [GNU Make の jobserver](https://www.gnu.org/software/make/manual/html_node/POSIX-Jobserver.html) を渡していれば、それと協調します。[`CARGO_MAKEFLAGS`](https://doc.rust-lang.org/cargo/reference/environment-variables.html) のような他のフラグも影響することがあります。jobserver が渡されていない場合、`rustc` は使用するジョブ数を自分で選びます。

Rust 1.76.0 以降、jobserver が利用可能に見えるのにアクセスできない場合、`rustc` は警告を出します。例:

```
$ echo 'fn main() {}' | MAKEFLAGS=--jobserver-auth=3,4 rustc -
warning: failed to connect to jobserver from environment variable `MAKEFLAGS="--jobserver-auth=3,4"`: cannot open file descriptor 3 from the jobserver environment variable value: Bad file descriptor (os error 9)
  |
  = note: the build environment is likely misconfigured
```

## ビルドシステムとの統合

### GNU Make

GNU Make から `rustc` を呼び出す場合、`Makefile` 内のすべての `rustc` 呼び出しを再帰的 (recursive) とマークすることが推奨されます（コマンドラインの先頭に `+` を付ける）。これにより GNU Make がそれらに対して jobserver を有効にします。たとえば:

```makefile
x:
	+@echo 'fn main() {}' | rustc -
```

再帰的な Make の中の `$(shell ...)` の中で `rustc` を呼び出す場合、`MAKEFLAGS` 変数をクリアすることで手動で jobserver を無効化できます。例:

```makefile
S := $(shell MAKEFLAGS= rustc --print sysroot)

x:
	@$(MAKE) y

y:
	@echo $(S)
```

### CMake

CMake 3.28 は [`add_custom_target`](https://cmake.org/cmake/help/latest/command/add_custom_target.html) コマンドで `JOB_SERVER_AWARE` オプションをサポートしています。例:

```cmake
cmake_minimum_required(VERSION 3.28)
project(x)
add_custom_target(x
    JOB_SERVER_AWARE TRUE
    COMMAND echo 'fn main() {}' | rustc -
)
```

それより古いバージョンで、CMake を Makefile ジェネレータと使う場合の回避策の1つは、コマンドのどこかに [`$(MAKE)`](https://www.gnu.org/software/make/manual/html_node/MAKE-Variable.html) を含めることで、GNU Make にそれを再帰的な Make 呼び出しとして扱わせることです。例:

```cmake
cmake_minimum_required(VERSION 3.22)
project(x)
add_custom_target(x
    COMMAND DUMMY_VARIABLE=$(MAKE) echo 'fn main() {}' | rustc -
)
```

---

本ページは [Jobserver (stable)](https://doc.rust-lang.org/stable/rustc/jobserver.html) の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
