---
title: Codegen オプション（要約）
---

# Codegen オプション（要約）

これらのオプションはすべて `-C` フラグ（"codegen" の略）を通じて `rustc` に渡されます。お使いのコンパイラ正確な一覧は `rustc -C help` で確認できます。

以下、各フラグを簡潔にまとめます（詳細な説明や個々の例は原文を参照）。

- **code-model** — コードモデルを選択する。`tiny`・`small`（多くのターゲットのデフォルト）・`kernel`・`medium`・`large`。アドレス範囲の制約に関わる。
- **codegen-units** — クレートを分割するコード生成単位の最大数（1以上の整数）。並列化で速くなるが生成コードは遅くなりうる。非インクリメンタルビルドのデフォルトは16、インクリメンタルビルドは256。
- **collapse-macro-debuginfo** — マクロ定義由来のコード位置を、デバッグ情報上でそのマクロの呼び出し元の単一位置へ畳み込むか制御する。`yes`/`no`/`external`（別クレート由来のマクロのみ畳み込む）。
- **control-flow-guard** — Windows の Control Flow Guard を有効にする（Windows 以外では無視される）。`yes`・`nochecks`（ランタイム強制なしでメタデータのみ）・`no`（デフォルト）。
- **debug-assertions** — `cfg(debug_assertions)` を有効/無効にする。未指定時は `opt-level=0` のときのみ自動で有効。
- **debuginfo** — デバッグ情報の生成レベル。`0`/`none`（デフォルト）、`line-directives-only`、`line-tables-only`、`1`/`limited`、`2`/`full`。`-g` フラグは `-C debuginfo=2` の別名。
- **default-linker-libraries** — リンカのデフォルトライブラリを含めるか（デフォルトは除外）。
- **dlltool** — `windows-gnu` ターゲットで、`raw-dylib` 用のインポートライブラリ生成に使う dlltool のパスを指定する。
- **dwarf-version** — 出力する DWARF のバージョン（2〜5）。プラットフォームごとにデフォルトが異なる。
- **embed-bitcode** — LLVM bitcode をオブジェクトファイルに埋め込むか（デフォルトは埋め込む）。LTO を使わないなら `no` にすると速くなる。LTO と `embed-bitcode=no` の組み合わせは起動時エラーになる。
- **extra-filename** — 各出力ファイル名に付加する接尾辞を指定する。
- **force-frame-pointers** — フレームポインタの使用を強制するか。
- **force-unwind-tables** — unwind テーブルの生成を強制するか。
- **incremental** — インクリメンタルコンパイルを有効にし、情報を保存するディレクトリを指定する。リリースビルドには非推奨（最適化が一部制限される）。
- **instrument-coverage** — instrumentation ベースのコードカバレッジを有効にする。詳細は[該当章](../instrument-coverage.html)。
- **jump-tables** — LLVM バックエンドが switch からジャンプテーブルを生成することを許可するか（デフォルトは許可）。JOP 攻撃対策として無効化されることがある。
- **link-arg** / **link-args** — リンカ呼び出しに追加の引数を1つ/複数追加する。
- **link-dead-code** — 通常は生成・リンクされないデッドコードも生成・リンクしようとする（デフォルトは無効）。古いコードカバレッジ計測のための機能で、使用は非推奨。
- **link-self-contained** — Rust に付属するライブラリ・オブジェクトを使うか、システムのものを使うか制御する。`linker` コンポーネント単位でも制御可能（`x86_64-unknown-linux-gnu` のみ安定）。
- **linker** — 使用するリンカの実行ファイルパスを指定する。
- **linker-features** — リンク時の機能を個別に有効/無効にする（例: `+lld`/`-lld`）。
- **linker-flavor** — リンカの種類を指定する（`gcc`・`ld`・`msvc`・`wasm-ld`・`ld64.lld`・`ld.lld`・`lld-link`・`em` など）。
- **linker-plugin-lto** — LTO の最適化をリンカに委ねる。詳細は[該当章](../linker-plugin-lto.html)。
- **llvm-args** — LLVM に直接渡す引数のリスト（安定性の保証対象外）。
- **lto** — リンク時最適化 (LTO) を制御する。`fat`（全クレートで最適化、デフォルト値）、`thin`（速いがほぼ同等の効果）、`no`（無効）。未指定時は codegen-units が1または opt-level=0 でない限り、ローカルクレートのみの thin LTO が行われる。
- **metadata** — シンボルマングリングに使うメタデータ文字列（空白区切り）を指定する。
- **no-prepopulate-passes** — パスマネージャに、通常の事前登録済みパスの代わりに空のパスリストを使わせる。
- **no-redzone** — レッドゾーンを無効化するか（デフォルトはターゲット依存）。
- **no-vectorize-loops** / **no-vectorize-slp** — ループベクトル化 / SLP ベクトル化を無効にする。
- **opt-level** — 最適化レベル。`0`（デフォルト、`debug_assertions` も有効化）、`1`、`2`、`3`、`s`（サイズ優先）、`z`（サイズをより優先）。`-O` は `opt-level=3` の別名。
- **overflow-checks** — 実行時の整数オーバーフローチェックを有効/無効にする。未指定時は `debug-assertions` に従う。
- **panic** — パニック時の挙動。`abort`（プロセス終了）、`immediate-abort`（パニックフックも呼ばない）、`unwind`（スタックを巻き戻す）。クレートグラフ内で `abort`/`immediate-abort` が使われる場合、最終バイナリも同じ戦略を使う必要がある。
- **passes** — 追加の LLVM パスをコンパイルに加える（安定性の保証対象外）。
- **prefer-dynamic** — 可能なら動的リンクを優先する（デフォルトは静的リンクを優先）。
- **profile-generate** / **profile-use** — PGO（プロファイルガイド最適化）用の計測バイナリ生成 / プロファイルデータの使用。詳細は[該当章](../profile-guided-optimization.html)。
- **relocation-model** — 位置独立コード (PIC) の生成を制御する。`static`（非再配置）、`pic`（完全な位置独立、多くのターゲットのデフォルト）、`pie`（シンボル差し替え非対応の位置独立実行ファイル）、その他 `dynamic-no-pic`・`ropi`・`rwpi`・`ropi-rwpi`・`default`。
- **relro-level** — RELRO（GOT を読み取り専用にするエクスプロイト対策）のレベル。`off`・`partial`・`full`。ELF 系ターゲットでは `full` がデフォルト。
- **remark** — 最適化パスのリマークを出力する（`all` で全パス）。
- **rpath** — バイナリに rpath を設定するか（Unix 系のみ有効。デフォルトは無効）。
- **save-temps** — コンパイル中に生成される一時ファイルを保持するか（デフォルトは削除）。
- **split-debuginfo** — デバッグ情報の分離方法。`off`（ELF・windows-gnu のデフォルト）、`packed`（Windows MSVC・macOS のデフォルト。`.pdb`/`.dSYM`/`.dwp` に分離）、`unpacked`（コンパイル単位ごとに分離）。
- **strip** — リンク時にデバッグ情報・シンボルを取り除くレベル。`none`（デフォルト）、`debuginfo`（デバッグ情報のみ除去）、`symbols`（シンボルテーブルも除去。バックトレースやプロファイリングに悪影響が出うる）。セキュリティ対策としては過信できない。
- **symbol-mangling-version** — シンボルマングリングの形式。現在は `v0` が選択可能。詳細は[Symbol Mangling の章](../symbol-mangling/index.html)。
- **target-cpu** — 特定の CPU 向けにコードを生成する。`native`（ホストの CPU）、`generic`（最小限の機能・現代的なチューニング）。候補は `rustc --print target-cpus` で確認できる。
- **target-feature** — ターゲットの個々のフィーチャーを `+`/`-` で有効/無効にする。unsafe であり、誤用すると[未定義の実行時挙動](../targets/known-issues.html)につながる。`+crt-static`/`-crt-static` で C ランタイムの静的リンクも制御できる。
- **tune-cpu** — 特定の CPU 向けにスケジューリングを最適化する（ABI・命令セットには影響しない）。unstable オプション。現時点では x86 ターゲットのみ有効。

---

本ページは [Codegen Options (stable)](https://doc.rust-lang.org/stable/rustc/codegen-options/index.html) の要約の非公式日本語訳です。各フラグの詳細な説明・例は [doc.rust-lang.org](https://doc.rust-lang.org/stable/rustc/codegen-options/index.html) を参照してください。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
