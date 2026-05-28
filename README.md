# verbalize

曖昧な違和感を構造化された問いに変換する言語化スキル（`verbalize`）と、
その出力を入力として自律的にPDCAサイクルを回すメタフレームワーク（`pdca-meta`）を
セットで提供する Claude Code プラグインです。

```
人間の思考フロー:  違和感 → 切り取り → [verbalize] → [pdca-meta] → 解決
                                       ^^^^^^^^^^^^^^^^^^^^^^^
                                       このプラグインの担当範囲
```

## 含まれるスキル

| スキル | 役割 |
|-------|------|
| `verbalize` | 「なんか変」「もやもや」といった曖昧な気づきを対話を通じて構造化された問いと評価基準に変換する |
| `pdca-meta` | 構造化された問いを受け取り、問の定義→工程構築→実行→評価のPDCAサイクルを自律的に回す。評価結果の蓄積からフレームワーク自体を改善する外側ループも備える |

`verbalize` の出力はそのまま `pdca-meta` の入力になります。
単独使用も可能です。

## インストール

Claude Code で以下のコマンドを実行してください。

```
/plugin marketplace add makotan/verbalize
/plugin install verbalize@verbalize-marketplace
```

ブランチやタグを固定する場合は `@ref` を付与します。

```
/plugin marketplace add makotan/verbalize@main
```

### アンインストール

```
/plugin uninstall verbalize@verbalize-marketplace
/plugin marketplace remove verbalize-marketplace
```

### 更新

```
/plugin marketplace update verbalize-marketplace
```

## 使い方

インストール後、スキルは以下の名前で呼び出せます。

```
/verbalize:verbalize    # 言語化スキル
/verbalize:pdca-meta    # PDCAメタフレームワーク
```

明示的に呼ばなくても、Claude が状況に応じて自動的に該当スキルを起動します。
たとえば「なんか変だと思った」「もやもやしている」と伝えると `verbalize` が、
「この問題をどう解くか考えて」「PDCAを回したい」と伝えると `pdca-meta` が呼ばれます。

### 典型的なフロー

1. 違和感や気づきを Claude に投げる
2. `verbalize` が対話を通じて問いと評価基準に整理する
3. その出力を `pdca-meta` に渡して解決プロセスを回す
4. 評価結果は `.pdca-data/` 配下に記録され、パターン集が育っていく

## 実行データの保存場所

プラグイン本体（配布物）と実行データ（ログ・パターン集）は物理的に分離されています。
実行データはプロジェクトルート配下の以下のディレクトリに保存されます。

```
{プロジェクトルート}/.pdca-data/
├── verbalize/
│   └── logs/verbalize_log.jsonl
└── pdca-meta/
    ├── guardrails.md         ← 人間が定義する制約条件
    ├── references/
    │   ├── patterns.jsonl
    │   └── eval_summary.json
    └── logs/
        ├── eval_log.jsonl
        └── meta_log.jsonl
```

初回利用時に `guardrails.md` のテンプレートが自動生成されます。
プロジェクトに応じた制約条件を記述してください。

`.pdca-data/` はリポジトリにコミットしないことを推奨します（本リポジトリの
`.gitignore` には既に追加済み）。

## ローカル開発

このリポジトリをクローンしてマーケットプレイスとして直接読み込めます。

```
git clone https://github.com/makotan/verbalize.git
cd verbalize
claude plugin validate .
```

Claude Code から：

```
/plugin marketplace add ./path/to/verbalize
/plugin install verbalize@verbalize-marketplace
```

## ディレクトリ構成

```
.claude-plugin/marketplace.json     ← マーケットプレイスカタログ
plugins/verbalize/
  .claude-plugin/plugin.json        ← プラグインマニフェスト
  skills/
    verbalize/SKILL.md              ← 言語化スキル
    pdca-meta/SKILL.md              ← PDCAメタフレームワーク
```

## ライセンス

MIT License — 詳細は [LICENSE](./LICENSE) を参照してください。
