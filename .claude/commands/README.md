# Future Product Lab Report System v1.2

展示会・海外視察・技術調査レポートを、単なる報告書ではなく、会社の知識資産へ育てるための Claude Code 用スキルセットです。

## 構成

```text
.claude/commands/
  make-report.md
  review-report.md
  publish-report.md
  archive-report.md
  build-report.md
  StarUpdate.md

  Config/
    FPL_STYLE.md
    report-config.yml

  Examples/
    SamplePrompt.md
```

## 推奨ワークフロー

```text
make-report.md
↓
review-report.md
↓
publish-report.md
↓
archive-report.md
```

一括実行する場合は `/build-report <フォルダー名>` を使う（上記4工程を順に統括実行する）。

`StarUpdate.md` は上記ワークフローの独立した1工程ではなく、**make-report・publish-report・archive-report それぞれの最後から自動的に呼び出される**、README.md の表示（`最終更新`列・★/◎/△バッジ）専任の更新エンジンである。表示だけをその場で洗い直したい場合は `/StarUpdate` として単体実行もできる。

## 各スキルの役割

### make-report.md

初稿作成。Nippou.txtと写真をもとにReport.mdを作る。作成・追記の最後に StarUpdate.md を実行し、README.mdの表示を更新する。Report.md冒頭には作成日・最終更新日を記入する。

### review-report.md

編集者。写真・本文・構成を整える。

### publish-report.md

出版社。GitHub公開前の品質保証を行う。README.mdの更新バッジ（★/◎/△）は StarUpdate.md に処理を委譲して洗い直す。あわせてReport.md冒頭の最終更新日をPUBLISH_SUMMARY.mdの実行日時と同期する。

### archive-report.md

知識アーキビスト。Report.mdから技術テーマ・企業・トレンド・アイデアを抽出し、`KnowledgeBase/` へ蓄積する。各ファイル冒頭には作成日・最終更新日を記入する。行の追加後、StarUpdate.md に処理を委譲してREADME.mdの表示を洗い直す。

### build-report.md

統括役。`Reports/<フォルダー名>/` を対象に、各工程を順番に実行する。差分ビルド（前回ビルド以降の変更分のみ処理）に対応。

### StarUpdate.md

README.md 全7テーブル（出張報告書・講演会レポート・Strategy・KnowledgeBase4種）の `最終更新`列・★/◎/△バッジ列だけを専任で洗い直すエンジン。判定ロジック（相対時間の表記・git履歴からの実質的な最終編集日の求め方・一括コミットの除外パターン等）はこのファイルにのみ実装されている。make-report・publish-report・archive-report の各工程末尾から自動的に呼ばれるほか、`/StarUpdate` で単体実行もできる。

## 配置

このリポジトリでは `.claude/commands/` 直下にフラットに配置しており、各ファイルがそのままスラッシュコマンド（`/make-report` 等）になります。

`Config/`・`Examples/` は補助資料であり、スキル本文から参照されます。

## 使い方

### 一括実行（推奨）

```text
/build-report 202604-HANNOVER
```

### 公開前チェックだけ行う場合

```text
publish-report.md のスキルを使って、現在の Report.md をGitHub公開前の品質で最終確認してください。画像リンク、Markdown表示、README.md、CHANGELOG.md、Release Notes、公開判定まで実施してください。
```

### 編集から公開まで行う場合

```text
review-report.md で Report.md を編集品質まで高めたあと、publish-report.md で公開前チェックを行ってください。
```

### 知識資産化まで行う場合

```text
build-report.md の考え方に従って、Report.md を公開品質に整えたうえで、archive-report.md により Knowledge、Companies、Trends、Ideas を更新してください。
```

### README.mdの表示（★/◎/△）だけを今すぐ更新したい場合

```text
/StarUpdate
```

## Version

v1.2
