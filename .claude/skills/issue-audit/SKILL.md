---
name: issue-audit
description: 整備 — open な Issue のラベル漏れ・階層ごとの適用違反・親子と Milestone の不整合を検査して直す
---

# Issue 整備

ルールの正本は `CLAUDE.md` の「ラベル設計」の表と「スプリント」。対象は open な Issue。

1. `gh issue list --state open --limit 200 --json number,title,labels,milestone` で全件を取る。
2. 各 Issue を次の違反で判定する。
   - `階層:` がちょうど1つ付いていない
   - 同じ軸（種別、優先度、状態、見積り）の値が2つ以上付いている
   - 表で「必須」の軸が欠けている、または「付けない」軸が付いている
   - エピックに Milestone が付いている
   - Milestone の有無と `状態:` が対応していない（Milestone 付きなのに状態が無い、Milestone が無いのに状態がある）
   - ストーリーとタスクで、親が無い（`gh api "repos/{owner}/{repo}/issues/<n>/parent"` が 404）。ただしストーリーの親エピックは任意なので報告に含めない
   - 見積りが `8` のストーリー（分割の検討を促す。違反ではなく報告のみ）
3. 結果を Issue 番号ごとの表で見せ、機械的に直せるもの（余分なラベルの除去、Milestone に対応する `状態: 着手待ち` の付与）と、判断が要るもの（種別・優先度・見積りの欠け、階層の誤り、親の欠け）に分ける。
4. 機械的に直せるものは `AskUserQuestion` で承認を得てから `gh issue edit <n> --add-label … --remove-label …` で直す。判断が要るものは、ユーザーに値を聞いてから直す。

完了条件: 取得した全 Issue を判定した。違反は 修正済み か 判断待ち（理由つき）のどちらかで、修正したものは同じコマンドで再取得して違反が消えている。
