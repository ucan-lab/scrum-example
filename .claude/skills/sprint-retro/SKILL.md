---
name: sprint-retro
description: スプリントのふりかえり。Keep・Problem・Try を記録し、改善アクションをストーリーにして、Milestone を閉じる
disable-model-invocation: true
---

# ふりかえり

`sprint-review` の後に実行する。

1. 対象の Milestone を決める（`gh api "repos/{owner}/{repo}/milestones?state=open" --jq '.[] | "\(.number) \(.title)"'`）。Milestone に open な Issue が残っていれば、`sprint-review` が終わっていないので止まってユーザーに伝える。
2. Milestone の説明からベロシティを読み、事実（ベロシティ、バックログへ戻した件数）をユーザーに示す。
3. Keep（続けること）、Problem（困ったこと）、Try（次に試すこと）をユーザーから聞く。Problem と Try は、対応する事実（Issue 番号など）があれば添える。
4. `docs/retrospectives/sprint-<N>.md` に、ベロシティ・Keep・Problem・Try を書く。
5. 各 Try について、ストーリーにするか見送るかを `AskUserQuestion` で決める。ストーリーにするものは `issue-create` で、`種別: 雑務` のストーリーとして起票する。起票した番号を 4 のファイルに追記する。
6. 承認を得てから Milestone を閉じる: `gh api -X PATCH "repos/{owner}/{repo}/milestones/<number>" -f state=closed`。

完了条件: `docs/retrospectives/sprint-<N>.md` に Keep・Problem・Try が書かれ、全ての Try が「起票した（番号つき）」か「見送った」のどちらかで、Milestone が closed になっている。
