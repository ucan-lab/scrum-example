---
name: sprint-review
description: スプリントレビュー。完成の定義でストーリーを判定し、ベロシティを記録し、未完了をバックログへ戻す
disable-model-invocation: true
---

# スプリントレビュー

完成の基準は `docs/definition-of-done.md`。Milestone と `状態:` の規則は `CLAUDE.md`。

1. `gh api "repos/{owner}/{repo}/milestones?state=open" --jq '.[] | "\(.number) \(.title) \(.due_on)"'` で対象を決める。複数あれば `AskUserQuestion` で選ぶ。
2. `gh issue list --milestone "<title>" --state all --limit 200 --json number,title,state,labels,body` で全件を取る。
3. 各ストーリーを完成の定義の項目ごとに判定する。受け入れ条件は本文の `- [ ]` が残っていないか、子タスクは `gh api "repos/{owner}/{repo}/issues/<n>/sub_issues" --jq '.[] | "\(.number) \(.state)"'`、PR は `gh pr list --state all --search "<n>" --json number,state,mergedAt` で確かめる。判定結果は「完了」「未完了（満たしていない項目）」のどちらかにする。
4. closed なのに完成の定義を満たしていないストーリーは、そのままユーザーに見せて扱いを聞く。
5. ベロシティ（完了したストーリーの `見積り:` の合計）、完了と未完了の一覧を報告する。
6. `AskUserQuestion` で承認を得てから、未完了のストーリーとその open なタスクを `gh issue edit <n> --remove-milestone --remove-label "状態: 着手待ち" --remove-label "状態: 進行中" --remove-label "状態: レビュー待ち" --remove-label "状態: ブロック中"` でバックログへ戻す。付いていない `状態:` ラベルの除去は無視してよい。次のスプリントに入れるかは `sprint-start` で決める。
7. Milestone の説明の末尾に `ベロシティ: <合計>` を追記する: `gh api -X PATCH "repos/{owner}/{repo}/milestones/<number>" -f description="<既存の説明>\n\nベロシティ: <合計>"`。

完了条件: Milestone 内の全ストーリーが「完了」か、バックログへ戻したかのどちらかで、Milestone の説明にベロシティが記録されている。Milestone はここでは閉じない（`sprint-retro` が閉じる）。
