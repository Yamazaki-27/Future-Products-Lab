# build-report.md

あなたは Bishamon Future Product Lab Report System のビルド責任者である。

このスキルは、展示会レポート作成の一連の流れを統括する。

---

## 引数の受け取り方

`/build-report <フォルダー名>` の形式で呼び出される。

例：`/build-report 202604-HANNOVER`

- 引数として渡されたフォルダー名をリポジトリ直下のフォルダーとして特定する
- 以降の全工程において、このフォルダーを作業対象とする
- フォルダーが存在しない場合は、そのことを報告して停止する

---

## 実行方針

各工程を順番にすべて実行する。

以下の停止を発生させない。

- 「次に進めますか？」
- 「実行してよいですか？」
- 「確認してください」

ただし、**停止条件**（後述）に該当した場合は例外とする。

---

## 工程と実行ファイル

以下の順番で実行する。各工程は、対応するスキルファイルを必ず読み込んでから実行する。

```
1. make-report
   スキルファイル: .claude/commands/make-report.md

2. review-report
   スキルファイル: .claude/commands/review-report.md

3. publish-report
   スキルファイル: .claude/commands/publish-report.md

4. archive-report
   スキルファイル: .claude/commands/archive-report.md
```

各スキルファイルを Read ツールで読み込み、その指示に完全に従って実行すること。
「相当の処理」と解釈して独自実装しない。必ずスキルファイルを読む。

各工程の許可設定（確認不要・即実行の範囲）はそれぞれのスキルファイルの指示に従う。
動画ファイルの削除（rm）を含むすべての操作も、各スキルファイルに許可が明記されていれば確認不要で実行する。

---

## 完了済み判定

各工程の実行前に、以下の手順で「前回ビルドから変更があるか」を確認する。

確認対象は `Report.md` だけでなく、**写真・動画・Nippou.txt** も含む。

```bash
# 作業対象フォルダー内で実行
# PUBLISH_SUMMARY.md より新しいファイルが1つでもあれば「変更あり」
newer=$(find . \
  \( -name "Report.md" \
  -o -name "Nippou.txt" \
  -o -path "*/Images/*" \) \
  -newer PUBLISH_SUMMARY.md 2>/dev/null | head -1)

# $newer が空なら「変更なし」、空でなければ「変更あり」
```

**変更なし**と判定された場合（`PUBLISH_SUMMARY.md` が存在し、上記のファイルがすべて `PUBLISH_SUMMARY.md` 以前のタイムスタンプ）：

- review-report・publish-report・archive-report をすべてスキップする
- 以下を出力して終了する：

```text
前回ビルドから変更なし。スキップします。

対象フォルダー：<フォルダー名>
PUBLISH_SUMMARY.md 更新日時：<日時>

再ビルドが必要な場合は、Report.md・Nippou.txt を編集するか、
Images/ に写真を追加してから再実行してください。
```

**変更あり**と判定された場合（`PUBLISH_SUMMARY.md` が存在しない、または上記ファイルのいずれかが `PUBLISH_SUMMARY.md` より新しい）：

- 変更があったファイルを特定してログに残す
- 通常通り各工程を実行する

---

## 工程の実行条件

### 1. make-report

以下のいずれかに該当する場合に実行する。

- `Report.md` が存在しない（初稿作成モード）
- `Nippou.txt` が `PUBLISH_SUMMARY.md` より新しい（追記モード）
- `Images/` に `PUBLISH_SUMMARY.md` より新しいファイルがある（追記モード）
- 明示的に初稿作成・再生成を求められた

追記モードの場合、**make-report.md の指示に従い、既存の `Report.md` の内容を変更せず、追加分のみ末尾に追記する。**

目的。

- Nippou.txt を中心に初稿作成（または追記）
- 写真を時系列で配置（または追加）
- 展示会概要を補足
- README.md へ追加

### 2. review-report

`Report.md` が存在する場合に実行する（make-report を実行した直後を含む）。

完了済み判定で「変更なし」と判定された場合はスキップする。

目的。

- 写真と本文の整合性確認
- 写真の向き補正
- 文章・構成の改善
- edit_log.md 更新

### 3. publish-report

review-report が完了した後に実行する。

完了済み判定で「変更なし」と判定された場合はスキップする。

目的。

- Markdown 品質確認
- GitHub 表示確認
- 画像リンク確認
- README 確認
- CHANGELOG 更新
- Release Notes 生成
- 公開判定

### 4. archive-report

publish-report の判定が `Ready for Publish` の場合のみ実行する。

`Not Ready` または `Needs Review` の場合は archive-report をスキップし、その理由を `build_log.md` に記録する。

完了済み判定で「変更なし」と判定された場合はスキップする。

目的。

- 技術テーマ抽出
- Knowledge 更新
- Companies 更新
- Trends 更新
- Ideas 更新

---

## build_log.md

作業対象フォルダー直下（例：`202604-HANNOVER/build_log.md`）に作成・追記する。

記録する内容。

- 実行日時
- 実行した工程
- 成功・失敗
- 生成・更新したファイル
- 次に必要な作業

---

## 停止条件

以下の場合は、次工程へ進まず `build_log.md` に理由を記録して停止する。

- 引数のフォルダーが存在しない
- `Report.md` が存在せず、make-report に必要な情報（Nippou.txt・写真）もない
- 画像リンク切れが多数あり、自動修正不能
- publish 判定が `Not Ready`
- 事実確認が必要な未確認事項が多く、処理を続けると誤情報が残る

---

## エラー時の処理

停止条件に該当しないエラーが発生した場合。

1. 可能な範囲で代替手段を試す
2. 代替手段でも失敗した場合は、失敗内容を `build_log.md` に記録する
3. 次の工程へ進む

不明点がある場合も質問せず、`［要確認：〇〇が不明］` と記録して処理を継続する。

---

## コンテキスト窓について

make-report は大量のテキスト・写真処理を行うため、コンテキストを大量に消費する。

一括実行の途中でコンテキストが逼迫した場合は、その工程の完了をもって一旦区切りとする。
次のセッションで `/build-report` を再実行すると、`Report.md` が既に存在するため make-report がスキップされ、残りの工程から継続できる。

---

## 最終出力

全工程が終わった後、以下を出力する。

```text
Future Product Lab Report System build 完了

対象フォルダー：<フォルダー名>

実行工程：
- make-report:    done / skipped（理由）/ failed（理由）
- review-report:  done / skipped（理由）/ failed（理由）
- publish-report: Ready for Publish / Needs Review / Not Ready
- archive-report: done / skipped（理由）/ failed（理由）

要確認事項：
- （あれば列挙）

変更ファイル：
- （主要なものを列挙）

推奨コミットメッセージ：
git commit -m "docs(<フォルダー名>): <変更内容の要約>"
```
