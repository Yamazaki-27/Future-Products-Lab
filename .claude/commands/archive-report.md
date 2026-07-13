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
KnowledgeBase/Knowledge/
KnowledgeBase/Companies/
KnowledgeBase/Trends/
KnowledgeBase/Ideas/
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
KnowledgeBase/Knowledge/AGV/MagneticTape.md
KnowledgeBase/Knowledge/AMR/SLAM.md
KnowledgeBase/Knowledge/Vision/CameraAI.md
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

企業名・出展社名が出てきた場合、`KnowledgeBase/Companies/企業名.md` を作成・更新する。

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
KnowledgeBase/Trends/2026.md
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
KnowledgeBase/Ideas/AGV_next_generation.md
KnowledgeBase/Ideas/Lift_AI_maintenance.md
```

記載内容。

- アイデア概要
- 背景
- 展示会での観察
- 想定製品・用途
- 技術課題
- 次のアクション

## 冒頭の作成日・最終更新日（必須・Knowledge/Companies/Trends/Ideas共通）

README.md の知識ベース4テーブルの ★/◎/△ バッジは相対時間の自動判定であり、根拠がREADME.mdからは見えない。信憑性を持たせるため、Knowledge・Companies・Trends・Ideasの各ファイルの**H1タイトルの直後**に作成日・最終更新日を記す。

```markdown
# ファイルタイトル

> 作成日：YYYY-MM-DD　最終更新日：YYYY-MM-DD

## 概要
...
```

- **新規作成時**：作成日・最終更新日の両方に本日の日付（`date "+%Y-%m-%d"`）を入れる
- **既存ファイルへの追記時**：既存の `> 作成日：` 行を探し、作成日はそのまま変更せず、最終更新日だけを本日の日付に書き換える。行が見つからない場合のみ新規に挿入する
- この行は独立した1行の `> 作成日：` ブロック引用として置く。他の記載内容と混在させない

## 写真挿入ルール

各 `.md` ファイルに、トピックに直接関係する写真を **3〜5枚** 挿入する。
写真は本文の流れに沿って各セクション内に分散配置する（概要に1枚・観察内容に1〜2枚・示唆・関連に1〜2枚）。

### STEP 1：候補写真の列挙

まず `grep` でキャプション行を絞り込み、候補一覧を作る。

```bash
# 企業名・技術名でキャプションを検索
grep -n "企業名\|技術キーワード" Report.md | grep "p style"
```

- キャプション行（`<p style=...>`）の**直前の `<img src=...>` 行**がその写真のファイル名
- OtherPictures/ の写真も対象に含める

### STEP 2：写真の適切性を判断する

候補写真のキャプションを読み、以下の基準で採否を判定する。

**採用する写真（トピックに直接関係するもの）：**
- そのブース・企業・製品が直接写っている
- キャプションにその企業名・技術名・製品名が明記されている
- 製品の動作・仕様・用途が視覚的に伝わる

**採用しない写真（的外れなもの）：**
- キャプションに対象企業・技術名が含まれていない
- 隣のブース・別の企業・関係ない展示が写っている
- 会場全景・人物のみ・食事・移動中など、トピックと無関係な内容
- 「近くにあった」「同じ日に撮った」だけの写真

**判断が難しい場合：** キャプションを声に出して読み、「この写真はこのファイルのテーマを説明しているか」を問う。Noなら採用しない。

### STEP 3：3〜5枚を本文に分散配置する

写真は1か所にまとめず、関連するセクションの直後に個別に挿入する。

```
## 概要         → 最も代表的な写真 1枚（ブース全景・製品全体）
## 観察内容     → 製品の詳細・デモの様子など 1〜2枚
## 技術的示唆   → 比較・参考になる写真 1枚（あれば）
## スギヤスへの示唆 → 応用イメージに近い写真（あれば）
```

### STEP 4：写真が足りない場合

**目標は最低3枚。**

同じ企業・技術のキャプションを持つ写真が3枚未満の場合：
- OtherPictures/ も含めて再検索し、3枚に達するまで探す
- それでも3枚に届かない場合のみ、ある分だけ挿入する（2枚以下でも可）
- **関係のない写真を水増しのために使わない**

### パスの書き方

| ファイル配置 | 採用写真（Images/直下）| OtherPictures の写真 |
|---|---|---|
| `KnowledgeBase/Knowledge/カテゴリ/ファイル.md` | `../../../Reports/展示会フォルダ/Images/` | `../../../Reports/展示会フォルダ/Images/OtherPictures/` |
| `KnowledgeBase/Companies/企業名.md` | `../../Reports/展示会フォルダ/Images/` | `../../Reports/展示会フォルダ/Images/OtherPictures/` |
| `KnowledgeBase/Trends/年.md` | `../../Reports/展示会フォルダ/Images/` | `../../Reports/展示会フォルダ/Images/OtherPictures/` |
| `KnowledgeBase/Ideas/アイデア名.md` | `../../Reports/展示会フォルダ/Images/` | `../../Reports/展示会フォルダ/Images/OtherPictures/` |

### 挿入フォーマット

**単独（800幅）：**
```markdown
<br>
<img src="パス/ファイル名.jpg" width="800">
<p style="color:#888888; font-size:1.05em;">キャプション（何が写っているか・展示会名）</p>
```

**2枚横並び（390幅）：**
```markdown
<br>
<p>
<img src="パス/ファイルA.jpg" width="390">
<img src="パス/ファイルB.jpg" width="390">
</p>
<p style="color:#888888; font-size:1.05em;">（左）説明。（右）説明。</p>
```

### 【重要】キャプション内でレポートにリンクする場合は `<a>` タグを使う

キャプションの展示会名（例：MODEX 2026）からレポート本体にリンクを張りたい場合、**Markdownのリンク記法 `[text](url)` を使ってはいけない**。

- `<p style=...>...</p>` は CommonMark/GFM 上「生のHTMLブロック」として扱われ、その内側のテキストはMarkdownとして再解釈されない
- そのため `<p>...（[MODEX 2026 Report.md](../../../Reports/展示会フォルダ/Report.md)）...</p>` のように書くと、リンクにならず `[MODEX 2026 Report.md](...)` という文字列がそのまま表示されてしまう
- 正しくは同じ行内で `<a href="...">...</a>` を使う

```markdown
<p style="color:#888888; font-size:1.05em;">キャプション本文（<a href="../../../Reports/展示会フォルダ/Report.md">MODEX 2026 Report.md</a>）</p>
```

（`## 関連レポート`セクションなど`<p>`タグの外側にある通常のMarkdownリンクはこの制約を受けないため、`[text](url)`のままでよい）

## 重要ルール

- Report.mdに書かれていない事実を創作しない
- 推測は推測として明記する
- 既存ファイルは削除しない
- 既存知識に追記する
- 同じ内容を重複追記しない
- 関連レポートへのリンクを必ず残す

## archive_log.md

作業履歴として `Reports/archive_log.md` を追記更新する。

記載内容。

- 実行日時
- 更新したKnowledgeファイル
- 更新したCompaniesファイル
- 更新したTrendsファイル
- 更新したIdeasファイル
- 抽出した重要テーマ
- 次回深掘り候補

### ファイルパスの書き方（クリック可能にする）

記載するファイルパスは、単なるコードスパン（`` `path` ``）ではなく、**必ずMarkdownリンクにする**。人間はどうしてもクリックしたくなるため、反応しないコードスパンのままにしない。

`Reports/archive_log.md` からの相対パスは以下の通り（archive_log.md自身が `Reports/` 直下にあるため）。

| 対象 | 書き方 |
|---|---|
| `KnowledgeBase/...` | `` [`KnowledgeBase/...`](../KnowledgeBase/...) `` |
| `Reports/展示会フォルダ/Report.md` | `` [`Reports/展示会フォルダ/Report.md`](展示会フォルダ/Report.md) `` |
| `README.md` | `` [`README.md`](../README.md) `` |

例。

```markdown
### 実行ファイル
[`Reports/202604-MODEX/Report.md`](202604-MODEX/Report.md)

| ファイル | 追記内容 |
|---|---|
| [`KnowledgeBase/Companies/BIC.md`](../KnowledgeBase/Companies/BIC.md) | ... |
```

## README.md 知識ベース目次の更新

archive_log.md の更新が終わったら、リポジトリのトップページ `README.md` の「知識ベース」セクションを更新する。

### 更新ルール

- README.md に `## 知識ベース（Knowledge Base）` セクションが存在する場合は上書き更新する。
- セクションが存在しない場合は新規作成し、Strategy テーブルの直後（`<br>` と `---` 区切りの前）に挿入する。
- 4カテゴリ（KnowledgeBase/Knowledge・KnowledgeBase/Companies・KnowledgeBase/Trends・KnowledgeBase/Ideas）それぞれの現存 `.md` ファイルを `find` で列挙し、テーブル形式でリンクを生成する。
- ファイルは `KnowledgeBase/Knowledge/AMR/*.md` のようにカテゴリ名を先に表示し、ファイル名は日本語タイトルに変換できる場合は変換する。
- 概要列は各ファイルの先頭数行（`##` 見出し・本文第一段落）から自動抽出する。

### テーブルフォーマット

**列順のルール：`最終更新`（相対時間テキスト）を一番左、バッジ（★◎△）をその次に置く。** この列順は README.md の全テーブル（出張報告書・講演会レポート・Strategy・知識ベース4種）で統一する。新規に行を追加する際は、この2列は `—` / 空欄のプレースホルダーで置く（実際の値は後述の StarUpdate.md が埋める）。

**技術テーマ（KnowledgeBase/Knowledge/）**

```markdown
| 最終更新 |  | カテゴリ | ファイル | 概要 |
|:---:|:---:|---|---|---|
| — |  | AMR | [ファイルタイトル](KnowledgeBase/Knowledge/AMR/Commoditization.md) | 概要（1行）|
```

**企業情報（KnowledgeBase/Companies/）**

```markdown
| 最終更新 |  | 企業名 | ファイル | 関係性 |
|:---:|:---:|---|---|---|
| — |  | 企業名 | [企業名.md](KnowledgeBase/Companies/企業名.md) | 担当・関係性 |
```

**トレンド（KnowledgeBase/Trends/）**

```markdown
| 最終更新 |  | ファイル | 内容 |
|:---:|:---:|---|---|
| — |  | [タイトル](KnowledgeBase/Trends/ファイル.md) | 概要（1行）|
```

**アイデア（KnowledgeBase/Ideas/）**

```markdown
| 最終更新 |  | ファイル | アイデア概要 |
|:---:|:---:|---|---|
| — |  | [タイトル](KnowledgeBase/Ideas/ファイル.md) | 概要（1行）|
```

**Strategy（strategy/ ・ README.md「## Strategy」節）／講演会レポート（README.md「## 講演会レポート」節）**

この2テーブルは archive-report の抽出対象ではないため、archive-report.md 自身は行を追加・編集しない。StarUpdate.md が既存行の最終更新・バッジのみを洗い直す対象である。

### 最終更新列・★/◎/△バッジ列（StarUpdate.md へ委譲）

新規に追加した行、および既存の全行の `最終更新`・バッジ列を実際の値に更新する処理は `StarUpdate.md`（`.claude/commands/StarUpdate.md`）に一元化されている。判定ロジック（相対時間の表記ルール・git履歴からの実質的な最終編集日の求め方・一括リファクタリングコミットの除外パターン等）はすべて `StarUpdate.md` にのみ存在する。archive-report.md はここに複製しない。

- 知識ベース4テーブルの行を追加・更新し終えたら、Read ツールで `StarUpdate.md` を読み込み、その指示に従って実行する
- Strategy・講演会レポートテーブルも同じタイミングで StarUpdate.md により洗い直される（archive-report.md 自身は関与しない）
- バッジ判定の除外パターンを追記する必要が生じた場合、追記先は `StarUpdate.md` のみとする

## README.md 出張報告書テーブルの「ナレッジ化」列更新

知識ベースへの追加が完了したら、README.md の「出張報告書」テーブルの「ナレッジ化」列を更新する。

### 更新ルール

- 対象レポートの行の「ナレッジ化」セルを `[M/D](Reports/archive_log.md)` 形式で埋める（例：`[7/2](Reports/archive_log.md)`）
- 既に日付が入っている場合は、追補があれば `[7/2・7/3](Reports/archive_log.md)` のように追記する
- `—`（未実施）の行はそのままにする

### テーブルフォーマット例

`最終更新`・バッジ列（一番左2列）は StarUpdate.md が管理する（出張報告書テーブルは PUBLISH_SUMMARY.md基準）。archive-report.md は「ナレッジ化」列のみを更新し、他の列には触れない。

```markdown
| 最終更新 |  | 日付 | 出張先 | 参加者 | 写真枚数・容量 | ナレッジ化 |
|:---:|:---:|---|---|---|:---:|:---:|
| Yesterday | ◎ | 2026年4月13日 | [🇺🇸 米国／MODEX 2026...](Reports/202604-MODEX/Report.md) | 山崎・橋本GM | 111枚・15.0MB | [7/2](Reports/archive_log.md) |
```

## 最終出力

最後に以下を報告する。

```text
archive-report.md による知識資産化を完了しました。

更新したカテゴリ：
- KnowledgeBase/Knowledge/AGV
- KnowledgeBase/Companies/〇〇
- KnowledgeBase/Trends/2026
- KnowledgeBase/Ideas/〇〇

主な抽出テーマ：
- ...

README.md の「知識ベース」セクションを更新しました。
```

