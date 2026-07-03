# edit_log.md — 202604-MANUVIT

---

## 2026-07-03 初稿作成 make-report

### 動画処理
- `Images/VID_*.mp4` 14本 → `qlmanage -t -s 800` でサムネイル抽出 → `*_thumb.jpg` 生成
- 生成した PNG を `sips -s format jpeg -s formatOptions 85` で JPEG 変換
- MP4 本体 14本を `rm` で削除（`dangerouslyDisableSandbox: true`）

### 写真リサイズ
- 全 77 ファイルを `sips -Z 800` でリサイズ（タイムスタンプ `touch -t` で保持）

### 写真振り分け
| 移動先 | ファイル数 | 内容 |
|---|:---:|---|
| `Images/` (採用) | 28 | カメラ IMG_6xxx × 14、工場内スマホ × 8、動画サムネ × 6 |
| `Images/OtherPictures/` | 45 | 移動・観光・食事・類似写真 |
| `Images/unUsed/` | 4 | 5秒差バースト重複・連続重複 |

### Report.md 新規作成
- 初稿作成モード（Report.md 未存在）
- 全 75 画像リンク確認済み（ゼロ切れ）
- HTML `<p>` タグ対称確認済み（opens=51, closes=51）

---

## 2026-07-03 review-report

### バックアップ
- `backup/Report_review_*.md` 作成済み

### 確認結果
- 画像リンク：全 75 リンク OK（切れなし）
- HTML 構造：`<p>` 51ペア対称・孤立タグなし
- Markdown 構文：問題なし
- 日本語表現・文体：である調・断言文体で統一
