---
title: ソースパスの置き換え
---

# ソースパスの置き換え（要約）

## 目的

`--remap-path-prefix` は、コンパイラの出力（診断、デバッグ情報、マクロ展開、オブジェクトファイル）内のソースパスの接頭辞を置き換えます。主な用途は、再現可能なビルド (reproducible build) と、ローカルの機密なパスを取り除くことによるプライバシー・セキュリティの確保です。

## 構文

```
rustc --remap-path-prefix FROM=TO
```

- `FROM`: 置き換え対象のパス接頭辞（`=` を含んでもよい）
- `TO`: 置き換え後のテキスト（`=` を含んではいけない）
- 置き換えは**純粋にテキストとして**行われ、パスの正規化は行われない
- 複数のルールが一致する場合、**最後のものが勝つ**

### 例

```bash
rustc --remap-path-prefix "/home/user/project=/redacted"
```

## 範囲の制御

`--remap-path-scope` は、コンパイラのどの出力を置き換え対象にするかを指定します。

| スコープ | 効果 |
|-------|--------|
| `macro` | `std::file!()` マクロの展開（パニックメッセージなど） |
| `diagnostics` | コンパイラのエラー・警告メッセージ |
| `debuginfo` | デバッグシンボル |
| `coverage` | カバレッジ情報 |
| `object` | 実行可能ファイル・ライブラリ内のすべてのパス（`macro,coverage,debuginfo` の別名） |
| `all` | **デフォルト** — unstable なものも含むすべてのスコープ |

**メタデータに関する注意**: rustc のメタデータ（`.rmeta`、`.rlib`、dylib）は、正しさのためにローカルパスを保持します。`all` スコープのみが、メタデータに対してベストエフォートで置き換えを適用します。

## 主な制約

- **リンカが生成するパス**（Windows の `.pdb`、Apple の OSO エントリなど）は影響を受けません
- **テキストのみ**であり、パス区切り文字（`/` と `\`）を賢く扱いません
- **外部ツール** — ビルドスクリプトや環境変数からのパスは置き換えられない場合があります

---

本ページは [Remap source paths (stable)](https://doc.rust-lang.org/stable/rustc/remap-source-paths.html) の要約の非公式日本語訳です。原文の著作権は The Rust Project Developers に帰属し、MIT / Apache-2.0 のデュアルライセンスで提供されています。
</content>
