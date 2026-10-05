# tdocsjp/rust

Rust 公式ドキュメントの非公式日本語訳。[tdocsjp/website](https://github.com/tdocsjp/website) の submodule として取り込まれる。

フォルダはチャンネル別（`stable/` など）に分ける。将来 beta/nightly を追加する場合もこの下に並べる。各クレートのページは rustdoc のパスをそのままミラーする（`struct.Foo.html` → `struct.Foo.md` など）。

- 原文: https://doc.rust-lang.org/ （stable チャンネル）
- ライセンス: 原文は MIT / Apache-2.0 のデュアルライセンス。著作権は The Rust Project Developers に帰属する
- 本訳は非公式であり、Rust プロジェクトによる承認を受けたものではない

## 翻訳済み

- `stable/index.md` — https://doc.rust-lang.org/stable/
- `stable/proc_macro/*` — 全27ページ（クレート索引・モジュール2つ・型/トレイト/関数/マクロ24項目）を完訳
- `stable/std/index.md` — std クレートのトップページ
- `stable/std/option/*`、`stable/std/result/*` — モジュール概要を全文翻訳（`Option`・`Result` 型の説明・慣用句・比較演算子など）
- `stable/std/vec/*`、`stable/std/string/*` — モジュール概要 + 型の概要（メモリレイアウト・容量などの説明）を全文翻訳
- `stable/std/collections/{struct.HashMap,hash_map/index}.md` — 要約
- `stable/std/iter/trait.Iterator.md`、`stable/std/boxed/struct.Box.md`、`stable/std/rc/struct.Rc.md`、`stable/std/sync/struct.{Arc,Mutex}.md` — 要約

いずれも、各メソッド単位の個別ドキュメント（100件超/型）は未着手。`Option`・`Result`・`Vec`・`String`・`HashMap` はモジュール/型の概要レベルでは完結しているが、`unwrap`・`push` のような個々のメソッドの説明・例は原文へのリンクのみ。

## 未翻訳

`std` には上記以外に約2,000ページ（struct 546・fn 594・trait 233 など）があり、proc_macro（27ページ）の70倍以上の規模。必要になった型・ページから追加していく。
</content>
