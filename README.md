# Scrum Example

## プロダクトゴール

GitHub の Issue・ラベル・Milestone と Claude のスキルでスクラム開発を 1 スプリント回せる、GitHub Projects テンプレートとして再利用できるサンプルにする。

## 含まれるもの

| 内容 | 場所 |
|---|---|
| ラベル定義（階層、種別、優先度、状態、見積りの5軸） | `.github/labels.yml` |
| Issue テンプレート（エピック・ストーリー・タスク） | `.github/ISSUE_TEMPLATE/` |
| PR テンプレート | `.github/pull_request_template.md` |
| Claude スキル（ラベル同期、スプリントの開始、レビュー、ふりかえり、Issue の起票と整備） | `.claude/skills/` |
| 完成の定義 | `docs/definition-of-done.md` |

ラベルの使い分けとスプリントの回し方は `CLAUDE.md` にまとめている。
