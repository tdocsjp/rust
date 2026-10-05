# tdocsjp/rust

Rust 公式ドキュメントの非公式日本語訳。[tdocsjp/website](https://github.com/tdocsjp/website) の submodule として取り込まれる。

フォルダはチャンネル別（`stable/` など）に分ける。将来 beta/nightly を追加する場合もこの下に並べる。各クレートのページは rustdoc のパスをそのままミラーする（`struct.Foo.html` → `struct.Foo.md` など）。

- 原文: https://doc.rust-lang.org/ （stable チャンネル）
- ライセンス: 原文は MIT / Apache-2.0 のデュアルライセンス。著作権は The Rust Project Developers に帰属する
- 本訳は非公式であり、Rust プロジェクトによる承認を受けたものではない

## 翻訳済み

### 概要ページ
- `stable/index.md` — https://doc.rust-lang.org/stable/
- `stable/std/index.md` — std クレートのトップページ

### proc_macro（全27ページ完訳）
`stable/proc_macro/**` — クレート索引・モジュール2つ（`token_stream`・`tracked`）・型/トレイト/関数/マクロ24項目のすべてを全文翻訳。

### std のよく使う型・トレイト（モジュール/型の概要レベル）
| 分類 | パス |
|---|---|
| Option / Result | `std/option/*`、`std/result/*`（全文翻訳） |
| Vec / String | `std/vec/*`、`std/string/*`（全文翻訳） |
| HashMap / HashSet / BTreeMap / VecDeque | `std/collections/*`（要約） |
| Box / Rc / Arc | `std/boxed/struct.Box.md`、`std/rc/struct.Rc.md`、`std/sync/struct.Arc.md`（要約） |
| Mutex / RwLock | `std/sync/struct.{Mutex,RwLock}.md`（要約） |
| Cell / RefCell | `std/cell/struct.RefCell.md`（要約） |
| Cow | `std/borrow/enum.Cow.md`（要約） |
| Iterator | `std/iter/trait.Iterator.md`（要約） |
| Path / PathBuf | `std/path/struct.Path.md`（要約） |
| Duration / Instant | `std/time/struct.{Duration,Instant}.md`（要約） |
| thread | `std/thread/index.md`（要約） |
| Error | `std/error/trait.Error.md`（要約） |
| From / Into | `std/convert/trait.From.md`（要約） |
| Default | `std/default/trait.Default.md`（要約） |

「全文翻訳」は原文の段落・コード例をそのまま翻訳したもの。「要約」は要点を日本語でまとめたもの（コード例は代表的なものに絞っている）。どちらも、各メソッド単位（`push`・`unwrap`・`lock` など）の個別の説明・例はまだなく、型/トレイトの概要レベルで止まっている。原文の個別ページへのリンクを各ファイル末尾に記載。

## 未翻訳

上記以外に std には約2,000ページ（struct 546・fn 594・trait 233 など）がある。候補: `BTreeSet`、`Weak`、`Cell`（RefCell とは別に単体のページ）、`Ordering`/`PartialOrd`/`Ord`、`Clone`、`Debug`/`Display`、`Drop`、`Deref`、`env`・`fs`・`io`・`process`・`net` モジュール、`char`・`str` のプリミティブページなど。必要になった型・ページから追加していく。
</content>
