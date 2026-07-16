# CHANGELOG

## 2026-07-16 14:56 Publish Check

### Fixed
- 縦位置写真4点（31.JPG・44.JPG・55.JPG・29_thumb.jpg）のwidth指定を800→500／400に修正

### Changed
- README.md「出張報告書」テーブルに本レポートの行を追加（make-report時点で実施済み）

### Checked
- Markdown構文（見出し階層・表・コードブロック）
- 画像リンク（本編64件・OtherPictures10件、リンク切れなし）
- 画像サイズ・容量（全て800px幅前後・7.9MB、GitHub掲載に適正）
- 写真の向き（EXIF orientation は全て`<nil>`だが目視確認により全て正常）
- GitHub表示互換（`<img>`・`<br>`・`<p>`・キャプションの`style`属性）
- 用語統一（全角英数字は直接引用の原文保持部分のみ残存。地の文には残存なし）

### Notes
- 引用文中の全角英数字（ＯＥＭ・６年経過後）は原文（Nippou.txt）どおりの表記であり、引用のため統一対象外
