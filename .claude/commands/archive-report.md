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

- 概要（文章）
- **代表写真（1枚以上）**：概要の直後に挿入（後述の「写真挿入ルール」参照）
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

## 写真挿入ルール

各 `.md` ファイルの「概要」セクションの直後（次の `##` 見出しの直前）に、そのトピックを代表する写真を **1枚以上** 挿入する。

### 写真の選び方

1. `Report.md` を `grep` して、そのファイルのテーマ（企業名・技術名）が含まれるキャプション行（`<p style=...>`）を特定する
2. キャプション行の直前にある `<img src="Images/..."` の行からファイル名を取得する
3. 同テーマの写真が複数ある場合は、最も典型的・わかりやすい1枚を選ぶ
4. 写真が1枚も見つからない場合は、関連する機器・会場・展示の写真を最も近いものを選ぶ

### パスの書き方

| ファイル配置 | 画像パスの先頭 |
|---|---|
| `Knowledge/カテゴリ/ファイル.md` | `../../202604-MODEX/Images/` |
| `Companies/企業名.md` | `../202604-MODEX/Images/` |
| `Trends/年.md` | `../202604-MODEX/Images/` |
| `Ideas/アイデア名.md` | `../202604-MODEX/Images/` |

### 挿入フォーマット

```markdown
## 概要

（概要テキスト）

<img src="パス/ファイル名.jpg" width="800">
<p style="color:#888888; font-size:1.05em;">キャプション（展示会名・観察内容を1〜2行で）（MODEX 2026）</p>

## 次のセクション見出し
```

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

## README.md 知識ベース目次の更新

archive_log.md の更新が終わったら、リポジトリのトップページ `README.md` の「知識ベース」セクションを更新する。

### 更新ルール

- README.md に `## 知識ベース（Knowledge Base）` セクションが存在する場合は上書き更新する。
- セクションが存在しない場合は新規作成し、Strategy テーブルの直後（`<br>` と `---` 区切りの前）に挿入する。
- 4カテゴリ（Knowledge・Companies・Trends・Ideas）それぞれの現存 `.md` ファイルを `find` で列挙し、テーブル形式でリンクを生成する。
- ファイルは `knowledge/AMR/*.md` のようにカテゴリ名を先に表示し、ファイル名は日本語タイトルに変換できる場合は変換する。
- 概要列は各ファイルの先頭数行（`##` 見出し・本文第一段落）から自動抽出する。

### テーブルフォーマット

**技術テーマ（Knowledge/）**

```markdown
| カテゴリ | ファイル | 概要 |
|---|---|---|
| AMR | [ファイルタイトル](Knowledge/AMR/Commoditization.md) | 概要（1行）|
```

**企業情報（Companies/）**

```markdown
| 企業名 | ファイル | 関係性 |
|---|---|---|
| 企業名 | [企業名.md](Companies/企業名.md) | 担当・関係性 |
```

**トレンド（Trends/）**

```markdown
| ファイル | 内容 |
|---|---|
| [タイトル](Trends/ファイル.md) | 概要（1行）|
```

**アイデア（Ideas/）**

```markdown
| ファイル | アイデア概要 |
|---|---|
| [タイトル](Ideas/ファイル.md) | 概要（1行）|
```

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

README.md の「知識ベース」セクションを更新しました。
```

