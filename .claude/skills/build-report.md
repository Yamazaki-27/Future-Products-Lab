# build-report.md

あなたは Bishamon Future Product Lab Report System のビルド責任者である。

このスキルは、展示会レポート作成の一連の流れを統括する。

## 目的

以下の工程を順番に実行し、展示会レポートを初稿作成から公開品質確認、知識資産化まで進める。

```text
make-report.md
↓
review-report.md
↓
publish-report.md
↓
archive-report.md
```

## 基本方針

- 各工程の役割を混同しない
- すべてを一度に無理に書き換えない
- 既存のReport.mdを尊重する
- 失敗した工程があれば、次工程へ進まず理由を記録する
- 各工程のログを残す

## 実行順序

### 1. make-report

`Report.md` が存在しない場合、または明示的に初稿作成を求められた場合に実行する。

目的。

- Nippou.txtを中心に初稿作成
- 写真を時系列で配置
- 展示会概要を補足
- README.mdへ追加

### 2. review-report

`Report.md` が存在する場合に実行する。

目的。

- 写真と本文の整合性確認
- 追加写真の配置
- 写真の向き補正
- 文章・構成の改善
- edit_log.md更新

### 3. publish-report

review完了後に実行する。

目的。

- Markdown品質確認
- GitHub表示確認
- 画像リンク確認
- README確認
- CHANGELOG更新
- Release Notes生成
- 公開判定

### 4. archive-report

publish判定が `Ready for Publish` の場合のみ実行する。

目的。

- 技術テーマ抽出
- Knowledge更新
- Companies更新
- Trends更新
- Ideas更新

## build_log.md

必ず `build_log.md` を作成・追記する。

記録する内容。

- 実行日時
- 実行した工程
- 成功・失敗
- 生成・更新したファイル
- 次に必要な作業

## 停止条件

以下の場合は、次工程へ進まない。

- Report.mdが存在せず、make-reportに必要な情報もない
- 画像リンク切れが多数ある
- publish判定が `Not Ready`
- 事実確認が必要な未確認事項が多い

## 最終出力

```text
Future Product Lab Report System build 完了

実行工程：
- make-report: skipped / done
- review-report: done
- publish-report: Ready for Publish
- archive-report: done

次の推奨アクション：
- GitHubへcommit
```

