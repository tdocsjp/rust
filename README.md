# tdocsjp/rust

Rust 公式ドキュメントの非公式日本語訳。[tdocsjp/website](https://github.com/tdocsjp/website) の submodule として取り込まれる。

フォルダはチャンネル別（`stable/` など）に分ける。将来 beta/nightly を追加する場合もこの下に並べる。各ページは mdBook / rustdoc のパスをそのままミラーする。

- 原文: https://doc.rust-lang.org/ （stable チャンネル）
- ライセンス: 原文は MIT / Apache-2.0 のデュアルライセンス。著作権は The Rust Project Developers に帰属する
- 本訳は非公式であり、Rust プロジェクトによる承認を受けたものではない

## 翻訳済み

### 概要ページ
- `stable/index.md` — https://doc.rust-lang.org/stable/
- `stable/std/index.md` — std クレートのトップページ

### proc_macro（全27ページ完訳）
`stable/proc_macro/**`

### std のよく使う型・トレイト（24種類、概要レベル）
`stable/std/{option,result,vec,string,collections,borrow,cell,sync,rc,boxed,iter,path,time,thread,error,convert,default}/**`

Option・Result・Vec・String は原文を全文翻訳。他（HashMap・HashSet・BTreeMap・VecDeque・Cow・RefCell・Rc・Arc・Mutex・RwLock・Box・Iterator・Path・Duration・Instant・thread・Error・From・Default）は要約。

### rustc book（本編25ページ）
`stable/rustc/**`

- `command-line-arguments*`・`jobserver` は全文翻訳（フラグのリファレンスのため）
- 他は要約: codegen オプション・lint・ターゲット・PGO・コードカバレッジ・linker-plugin LTO・check-cfg・ソースパス置き換え・エクスプロイト対策・シンボルマングリング・Platform Support・Target Tier Policy・貢献方法
- **未訳（原文へのリンクのみ）**: 個別プラットフォームページ（`platform-support/*`、130以上）、lint 個別一覧（504件）、v0 シンボル形式の文法仕様

「全文翻訳」は原文の段落・コード例をそのまま翻訳したもの。「要約」は要点を日本語でまとめたもの（コード例は代表的なものに絞っている）。いずれも各ファイル末尾に原文へのリンクを記載。

## 未翻訳

- std には上記以外に約2,000ページ（struct 546・fn 594・trait 233 など）がある
- The Book、Reference、Rust by Example、Nomicon、Clippy Book、Rustdoc Book、Style Guide、Edition Guide、Embedded Book、エラーコード一覧（518件）は未着手

必要になった型・ページ・本から追加していく。
