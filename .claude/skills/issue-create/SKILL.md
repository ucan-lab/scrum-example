---
name: issue-create
description: 起票 — エピック・ストーリー・タスクの Issue を、階層ラベル・必要なラベル・本文・親子のサブ Issue まで揃えて作る
---

# Issue 起票

ルールの正本は `CLAUDE.md` の「ラベル設計」の表。本文の項目は `.github/ISSUE_TEMPLATE/<level>.yml`（`epic` / `story` / `task`）。`gh issue create` は Issue フォームを読めないので、テンプレートの内容をここで再現する。

1. 階層を決める。指定が無ければ `AskUserQuestion` で聞く。
2. 本文を作る。テンプレートの `body` の各 `label` を `### <label>` 見出しにして並べる。`required: true` の項目は必ず埋め、足りない情報はユーザーに聞く。任意の項目で書くことが無ければ `_No response_` と書く。本文はスクラッチパッドのファイルに書き出す。
3. ラベルを決める。表で「必須」の軸だけを付け、「付けない」軸は付けない。決まらない値（種別、優先度、見積り）は `AskUserQuestion` で聞く。見積りが `8` なら、分割を提案してから進める。階層ラベルは自動で付かないので、必ず `--label` に含める。
4. 作る: `gh issue create --title "<title>" --body-file <ファイル> --label "階層: …" --label "…"`。出力の URL から番号を控える。
5. 親子を紐づける。タスクの親はストーリー、ストーリーの親はエピック。親が分かっていれば、子の id を `gh api "repos/{owner}/{repo}/issues/<子の番号>" --jq .id` で取り、`gh api "repos/{owner}/{repo}/issues/<親の番号>/sub_issues" -F sub_issue_id=<id>` で紐づける。親が決まっていないタスクは、ユーザーに親ストーリーを確認する。
6. 親ストーリーに Milestone が付いているタスクには、同じ Milestone と `状態: 着手待ち` を付ける（`gh issue edit`）。

完了条件: Issue が存在し、付いているラベルが表の通り（過不足なし）で、本文の見出しがテンプレートの項目と一致し、親が分かっているものはサブ Issue として紐づいている。作った Issue の URL を報告する。
