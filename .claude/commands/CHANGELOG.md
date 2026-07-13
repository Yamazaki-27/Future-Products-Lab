# CHANGELOG

## v1.2 - 2026-07-13

### Added
- StarUpdate.md を新規追加：README.md 全7テーブル（出張報告書・講演会レポート・Strategy・KnowledgeBase4種）の `最終更新`列・★/◎/△バッジ列の判定ロジックを一元化した単独スキル。`/StarUpdate` として単体実行できる
- make-report.md：Report.md 作成・追記の最後に StarUpdate.md を必ず実行するよう配線。レビュー・公開前チェックを待たずに、レポート作成直後からトップページの表示が最新化される
- make-report.md・archive-report.md：Report.md / KnowledgeBase各ファイル（Knowledge・Companies・Trends・Ideas）のH1直後に `> 作成日：YYYY-MM-DD　最終更新日：YYYY-MM-DD` を記入するルールを追加。README.mdのバッジ判定根拠がファイル本体からも確認できるようにし、信憑性を担保する
- publish-report.md（9.5節）：PUBLISH_SUMMARY.md 実行日時とReport.md冒頭の最終更新日を同期する処理を追加
- 既存の Report.md 19件・KnowledgeBase 68件に作成日・最終更新日ヘッダーを遡って追記（バックフィル）

### Changed
- publish-report.md（9.4節）：バッジ判定ロジックの実装を撤去し、StarUpdate.md への委譲に変更（ロジックの二重管理を解消）
- archive-report.md：`find_real_last_edit()` / `effective_time()` / `rel_time()` の実装およびバッジ列ロジックを撤去し、StarUpdate.md への委譲に変更。行内容（ナレッジ化列・テーブルへの行追加）の担当は従来どおり

### Notes
- 判定ロジック（相対時間の表記ルール・一括リファクタリングコミットの除外パターン等）の変更・追記は、今後 StarUpdate.md にのみ加える
- 作成日・最終更新日ヘッダーは Strategy（`strategy/`）・講演会レポート（`Reports/講演フォルダ/`）には未適用。これらは dx-strategy.md・infra-mente.md・make-lecture.md が個別に管理しており、既存の書式（`> 作成：...`）と競合するため今回は対象外とした

## v1.1 - 2026-07-09

### Added
- build-report.md：差分ビルドモード（前回ビルド以降の変更ファイルのみ処理、トークン消費最小化）〔7/6-7/7〕
- publish-report.md：README.md 更新バッジの自動更新（9.4節）〔7/8〕
- archive-report.md：KnowledgeBase 4テーブルのバッジ列・相対時間表記〔7/8〕
- archive-report.md：`effective_time()` — 未コミット編集分を mtime で判定（git履歴のみだと新規ファイルを誤判定するため）〔7/9〕

### Changed
- バッジ体系を3段階に変更：★=24時間以内・◎=3日以内・△=7日以内〔7/9〕（旧：★=3日以内・○=7日以内。○は視認性が低いため△へ変更）
- make-report.md：README 目次テーブルを6列形式（バッジ列＋ナレッジ化列）に更新
- build-report.md：対象フォルダーの場所を `Reports/` 直下に明記
- build-report.md：「変更なし」判定時の写真審査範囲を明確化（動画残存チェック・未分類確認のみ）
- publish-report.md：PUBLISH_SUMMARY.md 日付抽出に `head -1` を追加（差分ビルドの複数日付行対応）
- report-config.yml：KnowledgeBase パス・バッジ定義を現行構成に同期

### Fixed
- publish-report.md：末尾「コミットメッセージ形式」のコードフェンス閉じ忘れを修正（以降の見出しがコードブロックに飲み込まれていた）

## v1.0 - 2026-07-02

### Added
- publish-report.md を追加
- review-report.md を追加
- archive-report.md を追加
- build-report.md を追加
- FPL_STYLE.md を追加
- report-config.yml を追加
- README.md を追加

### Notes
- Future Product Lab Report System の初期バージョン。
