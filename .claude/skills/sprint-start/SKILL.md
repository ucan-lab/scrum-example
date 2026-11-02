---
name: sprint-start
description: 新しいスプリントの Milestone を作り、バックログのストーリーとそのタスクを入れる
disable-model-invocation: true
---

# スプリント開始

1スプリント = 1 Milestone（`スプリント N`）。ルールの正本は `CLAUDE.md` の「ラベル設計」と「スプリント」。

1. `gh repo view --json nameWithOwner --jq .nameWithOwner` でリポジトリを確かめる。リモートが無ければ止まる。
2. `gh api "repos/{owner}/{repo}/milestones?state=all" --jq '.[].title'` で既存の最大の N を取り、次の連番を決める。
3. `AskUserQuestion` で開始日と期間（1週 / 2週 / 他）を聞く。期日は 開始日 + 期間。
4. Milestone を作る: `gh api "repos/{owner}/{repo}/milestones" -f title="スプリント N" -f due_on=<YYYY-MM-DDT00:00:00Z> -f description="<スプリントゴール>"`。スプリントゴールはユーザーに聞く。
5. 候補を出す: `gh issue list --state open --label "階層: ストーリー" --search "no:milestone" --json number,title,labels`。優先度の高い順に番号つきで見せ、`AskUserQuestion` で入れるストーリーを決めてもらう（番号の指定は Other）。
6. 選んだ各ストーリーについて、`gh api "repos/{owner}/{repo}/issues/<n>/sub_issues" --jq '.[].number'` で子のタスクを取る。ストーリーとその全タスクに `gh issue edit <n> --milestone "スプリント N" --add-label "状態: 着手待ち"` を実行する。

完了条件: Milestone が存在し、選んだ全ストーリーとその全タスクに Milestone と `状態: 着手待ち` が付いている。最後に、入れたストーリーの `見積り:` の合計と、`見積り:` の無いストーリー（あれば番号）を報告する。
