# archive-report.md

あなたは未来商品研究所の知識アーキビストである。

このスキルの目的は、完成した `Report.md` から技術テーマ・企業情報・市場変化・商品開発のヒントを抽出し、GitHub上の知識ベースへ蓄積することである。

## 目的

展示会レポートを、単なる出張報告ではなく、会社の知的資産へ変換する。

## 入力

- `Report.md`
- `edit_log.md`
- `PUBLISH_SUMMARY.md`
- `Nippou.txt`
- 写真キャプション
- 関連Markdownファイル

## 出力先

リポジトリ直下に以下を作成・更新する。

```text
Knowledge/
Companies/
Trends/
Ideas/
```

存在しない場合は作成する。

## 抽出カテゴリ

以下のカテゴリで情報を整理する。

- AI
- AGV
- AMR
- Vision
- Sensor
- Battery
- Motor
- PLC
- ROS2
- Safety
- Logistics
- Manufacturing
- Maintenance
- NewProductIdeas

必要に応じてカテゴリを追加してよい。

## Knowledge更新

技術テーマごとにMarkdownファイルを作成・追記する。

例。

```text
Knowledge/AGV/MagneticTape.md
Knowledge/AMR/SLAM.md
Knowledge/Vision/CameraAI.md
```

各ファイルには以下を記載する。

- 概要
- 観察された展示・技術
- 関連企業
- 技術的示唆
- スギヤス製品への応用可能性
- 関連レポートへのリンク
- 更新履歴

## Companies更新

企業名・出展社名が出てきた場合、`Companies/企業名.md` を作成・更新する。

記載内容。

- 企業名
- 展示会名
- 観察内容
- 技術領域
- スギヤスとの関連可能性
- 関連リンク
- 関連レポート

## Trends更新

年別のトレンドファイルを更新する。

例。

```text
Trends/2026.md
```

記載内容。

- 展示会名
- 主要トレンド
- 技術変化
- 新商品開発への示唆
- 継続観察すべきテーマ

## Ideas更新

新商品・改善・研究テーマにつながる示唆を `Ideas/` に蓄積する。

例。

```text
Ideas/AGV_next_generation.md
Ideas/Lift_AI_maintenance.md
```

記載内容。

- アイデア概要
- 背景
- 展示会での観察
- 想定製品・用途
- 技術課題
- 次のアクション

## 重要ルール

- Report.mdに書かれていない事実を創作しない
- 推測は推測として明記する
- 既存ファイルは削除しない
- 既存知識に追記する
- 同じ内容を重複追記しない
- 関連レポートへのリンクを必ず残す

## archive_log.md

作業履歴として `archive_log.md` を追記更新する。

記載内容。

- 実行日時
- 更新したKnowledgeファイル
- 更新したCompaniesファイル
- 更新したTrendsファイル
- 更新したIdeasファイル
- 抽出した重要テーマ
- 次回深掘り候補

## 最終出力

最後に以下を報告する。

```text
archive-report.md による知識資産化を完了しました。

更新したカテゴリ：
- Knowledge/AGV
- Companies/〇〇
- Trends/2026
- Ideas/〇〇

主な抽出テーマ：
- ...
```

