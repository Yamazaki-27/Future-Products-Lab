# CHANGELOG

## v1.5 - 2026-07-22

### Changed
- StarUpdate.md（5.1節）：出張報告書テーブルの判定を「PUBLISH_SUMMARY.md の実行日時」単独から「実行日時と Report.md の実質的な最終編集（git履歴ベース）の新しい方」に変更。公開後に Report.md へ直接追記されたケース（例：202604-IMS 営業説明会での意見、7/21 淵田氏追記）が鮮度表示に反映されなかった問題への対応。PUBLISH_SUMMARY.md が無いレポートは従来どおり「—」

## v1.4 - 2026-07-21

### Added
- make-pdf.md を新規追加：Report.md を配布用PDF（通常版＋軽量版）に変換するスキル（Yamazaki-OS/Core/make-pdf.md と同内容）

### Changed（Yamazaki-OS 202607-Yatsugatake レポート作成時の知見を同期）
- make-report.md：キャプションは本文の文言をなぞらない（写真ならではの視点を足す。悪い例・良い例つき）
- make-report.md：縦横の判定は目視・エージェント報告に頼らず sips 実寸で全数確認
- make-report.md：横並びペアのキャプションは `</p>` の外側に置く（`<p>` 内側だとPDF変換で崩れる）
- make-report.md：AirDrop・共有シート経由のUUID名写真はEXIF撮影日時が共有時刻に置き換わっていることがある旨を追記
- review-report.md：確認ポイントに「キャプションが本文の文言をなぞっているだけになっていないか」を追加
- CLAUDE.md：画像ファイルの注意事項にキャプション規範と make-pdf.md への参照を追加
- CLAUDE.md：文体の特徴に「ボケ・セルフツッコミは歓迎」（のはウソで構文・真顔の自虐・キャプション芸・断言→留保のリズム）を追記
- make-report.md：キャプションルールに「キャプション芸の型」を追記

## v1.3 - 2026-07-21

### Changed
- make-report.md：写真の採否判断・重複判定・時系列配置を、タイムスタンプ単独からEXIFのGPS情報（撮影場所）併用に変更（Yamazaki-OS/Core/make-report.md にも同内容を反映）
  - 背景：多人数撮影では、同時刻でも別の撮影者が別の場所で撮った別の場面がありうる。また機器ごとの時計ズレで時刻順が実際の行動順と食い違うことがある
  - 「多人数撮影への対応：時刻とGPSを合わせて判定する」節を新設：`mdls`（または `exiftool`）で全写真の撮影日時・GPSを一覧化し、緯度経度差 約0.001度（約100m）以内を「同じ撮影地点」とみなす
  - 重複間引きの基準を「同じ時間帯」→「同じ時間帯・同じ撮影地点（GPS）」に変更。同時刻でもGPSが離れていれば別の場面として残す
  - 時計ズレが疑われる場合は、GPSが同一地点の写真を同じ場面としてまとめ、場面単位で配置する。GPS無し写真は従来どおりタイムスタンプと被写体内容で判断する

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
