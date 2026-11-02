# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

GitHub と Claude でスクラム開発をするサンプルプロジェクト。アプリケーションコードは持たず、次の3つをまとめる。

- GitHub のラベル設計
- Issue・PR テンプレート
- Claude スキル（リポジトリ管理用）

## ラベル設計

定義の正本は `.github/labels.yml`。名前は `軸: 値` の形式で、日本語。

- 階層（エピック > ストーリー > タスク）: ラベルで区別し、親子は GitHub のサブ Issue で紐づける。個人アカウントのリポジトリでは Issue Types が使えないためラベルで代用している
- 種別・優先度・状態・見積り: 階層ごとの適用先は次の表の通り
- 完了は `状態:` ラベルを作らず Issue の closed で表す

| 軸 | エピック | ストーリー | タスク |
|---|---|---|---|
| 階層 | 必須 | 必須 | 必須 |
| 種別 | 任意 | 必須 | 必須 |
| 優先度 | 必須 | 必須 | 付けない（親を継ぐ） |
| 見積り | 付けない（子の合計） | 必須 | 付けない |
| 状態 | 付けない | スプリント内のみ必須 | スプリント内のみ必須 |

`状態:` はスプリントに入った（Milestone が付いた）ストーリーとタスクに付ける。バックログには付けない。

## スプリント

1スプリント = 1 Milestone（名前は `スプリント 1` のような連番、期日で期間を表す）。ストーリーとタスクに付ける。エピックは複数のスプリントにまたがるので付けない。ラベルや Issue ではスプリントを表さない。

スプリントは `sprint-start` → 開発 → `sprint-review` → `sprint-retro` の順に回す。完成の定義は `docs/definition-of-done.md`、ふりかえりの記録は `docs/retrospectives/sprint-<N>.md`。

## State

初期状態で、ファイルは `README.md`（見出しのみ）しかない。ビルド・lint・テストのコマンドは無い。構成が固まったら、ラベル定義・テンプレート・スキルの置き場所と相互の関係をここに追記する。
