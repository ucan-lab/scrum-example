---
name: label-sync
description: .github/labels.yml のラベルを GitHub リポジトリに反映する
disable-model-invocation: true
---

# ラベル同期

`.github/labels.yml` を正本として、GitHub のラベルを揃える。

1. `gh repo view --json nameWithOwner --jq .nameWithOwner` でリポジトリを確かめる。リモートが無ければ、ここで止まってユーザーに伝える。
2. `labels.yml` の全ラベルについて、`gh label create "<name>" --color <color> --description "<description>" --force` を実行する（`--force` で既存は更新される）。
3. `gh label list --limit 200 --json name --jq '.[].name'` で現状を取り、`labels.yml` に無いラベルを一覧にしてユーザーに見せる。削除は `AskUserQuestion` で承認を得たラベルだけ `gh label delete "<name>" --yes` で行う。付いている Issue から外れるため、削除前に件数を添える。

完了条件: `labels.yml` の全ラベルが `gh label list` に存在し、色と説明が定義と一致している。定義に無いラベルは、削除したか、残すとユーザーが決めた。
