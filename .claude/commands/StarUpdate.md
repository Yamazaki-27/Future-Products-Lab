# StarUpdate.md — README.md 最終更新・★/◎/△バッジ 一括更新エンジン

あなたはトップページ（`README.md`）の鮮度表示だけを専門に担当する更新係である。

このスキルの唯一の仕事は、`README.md` の全テーブルにある **`最終更新` 列（相対時間テキスト）** と **バッジ列（★/◎/△）** を、実行時点の実態に合わせて洗い直すことである。

内容（行の追加・削除・リンク・本文テキスト・ナレッジ化列など）には一切触れない。

---

## 1. 位置づけ

以前は同じロジックが `publish-report.md`（9.4節）と `archive-report.md`（★/◎/△バッジ列節）に別々に実装されていた。判定ロジックが2箇所に分岐すると、片方だけ直して片方を直し忘れる事故が起きる。

このスキルが唯一の実装場所である。他のスキルはロジックを持たず、このスキルを呼び出すだけにする。

### 1.1 自動的に呼び出す側（このスキルを直接編集しなくても、以下を実行すれば結果的に必ず走る）

- **`make-report.md`**：Report.md の新規作成・追記が終わった直後（README.md に新しい行を追加した直後）に必ず実行する。これにより、レビュー・公開前チェックを待たずとも、作った瞬間にトップページの表示が最新化される。
- **`publish-report.md`**：9.4節から呼び出す。
- **`archive-report.md`**：知識ベース目次・ナレッジ化列の更新後に呼び出す。
- **`build-report.md`**：上記3スキルを内部で呼ぶため、`/build-report` 経由でも自動的に実行される。

### 1.2 単独実行

`/StarUpdate` とだけ入力すれば、他の工程を挟まずに README.md 全体（7テーブル）のバッジ・最終更新列だけをその場で洗い直せる。

用途の例。

- 他の作業（Git操作・ファイル整理等）でリポジトリの状態が変わった後、表示だけ最新化したい
- 「なぜかバッジが古いまま」という不整合に気づいたときの手動修正
- 一括リファクタリングコミットの直後に、除外パターンを追記してから再判定したい

---

## 2. 対象：README.md の7テーブル

| # | セクション | 見出し | 基準 |
|---|---|---|---|
| 1 | 出張報告書 | `## 出張報告書` | `PUBLISH_SUMMARY.md` の実行日時 |
| 2 | 講演会レポート | `## 講演会レポート` | git履歴（実質的な最終編集コミット） |
| 3 | Strategy | `## Strategy` | git履歴 |
| 4 | 知識ベース：技術テーマ | `### 技術テーマ（Knowledge/）` | git履歴 |
| 5 | 知識ベース：企業情報 | `### 企業情報（Companies/）` | git履歴 |
| 6 | 知識ベース：トレンド | `### トレンド（Trends/）` | git履歴 |
| 7 | 知識ベース：アイデア | `### アイデア（Ideas/）` | git履歴 |

**7テーブル全行を毎回洗い直す。** 今回どこかの工程が触ったファイルだけに限定しない（stale化防止）。テーブルやセクション自体が README.md に存在しない場合は、そのテーブルをスキップし作成はしない（テーブルの新規作成は make-report.md / archive-report.md の仕事）。

---

## 3. バッジ定義

`.claude/commands/Config/report-config.yml` の `publish.badges` を読む。

```yaml
publish:
  badges:
    within_24h: "★"
    within_3d: "◎"
    within_7d: "△"
```

設定が読めない場合のデフォルトは `★` / `◎` / `△`。絵文字（🆕等）は環境によって表示されないため使わない。

| 記号 | 意味 |
|:---:|---|
| ★ | 過去24時間以内に実質的な更新があった（最新） |
| ◎ | 過去3日以内（24時間超）に実質的な更新があった |
| △ | 過去7日以内（3日超）に実質的な更新があった |
| （空欄） | 7日を超えている、または実質的な編集履歴が無い |

列順：**`最終更新`（相対時間テキスト）を一番左、バッジをその次**に置く（`|:---:|:---:|...`）。バッジ記号だけでは何日前か分からず、リンクの後ろに文字として埋め込むと行ごとの長さで位置が揃わないため、必ず専用の2列にする。バッジ列の見出しは空欄（`|  |`）。

---

## 4. 共通ロジック（git履歴ベースの4〜7テーブルおよび2・3で使用）

### 4.1 なぜ生の `git log -1` をそのまま使わないか

このリポジトリには過去に733ファイル・1454ファイル等を同時に変更する一括リファクタリングコミット（フォルダ整理・リネーム等）が複数ある。単純に「そのファイルを最後に触ったコミット」を使うと、内容と無関係にほぼ全ファイルが「最近更新」と誤判定される。

そこで、**既知の機械的操作のコミットメッセージパターンを除外**し、**極端に大きい一括コミット（100ファイル超）**も安全側で除外する。

### 4.2 除外パターン（唯一のリスト。他ファイルに複製しない）

```bash
is_mechanical_commit() {
  local msg="$1"
  case "$msg" in
    refactor:*|"TopPageを見通しやすく整理"|"写真配置見直し"|"Folder-Rename"|"Fukushima"|"リポジトリ全てCore-Tec-R-+D-KAIZENへ移動") return 0 ;;
    *) return 1 ;;
  esac
}
```

将来また大規模なフォルダ整理・一括置換を行う場合、そのコミットメッセージが上記に該当しなければ、**このリストにだけ**追記する（publish-report.md・archive-report.mdには追記しない。二重管理を避けるため、追記対象は必ずこのファイルのみ）。

### 4.3 実質的な最終更新コミットを求める

```bash
find_real_last_edit() {
  local f="$1"
  for h in $(git log --follow --format="%H" -- "$f" 2>/dev/null); do
    msg=$(git log -1 --format="%s" "$h")
    is_mechanical_commit "$msg" && continue
    n=$(git show --stat "$h" 2>/dev/null | tail -1 | grep -oE "^[[:space:]]*[0-9]+" | tr -d ' ')
    [ -z "$n" ] && n=1
    if [ "$n" -le 100 ]; then
      git show -s --format="%ct" "$h"
      return
    fi
  done
}
```

### 4.4 未コミット編集分の優先（mtime）

実行中に作成・追記したばかりのファイルはまだ git 履歴に存在しない。`find_real_last_edit()` だけに頼ると古いコミット日時を拾って誤判定するため、`git status --porcelain` で変更中（modified / untracked）のファイルは mtime を優先する。

```bash
effective_time() {
  local f="$1"
  if [ -n "$(git status --porcelain -- "$f" 2>/dev/null)" ]; then
    stat -f "%m" "$f" 2>/dev/null
    return
  fi
  find_real_last_edit "$f"
}
```

### 4.5 相対時間テキスト（`最終更新`列）

```bash
rel_time() {
  local f="$1"
  local t; t=$(effective_time "$f")
  [ -z "$t" ] && t=$(git log --follow -1 --format="%ct" -- "$f" 2>/dev/null)
  [ -z "$t" ] && t=$(stat -f "%m" "$f" 2>/dev/null)
  [ -z "$t" ] && { echo "—"; return; }
  local diff=$(( $(date +%s) - t ))
  if   [ $diff -lt 60 ];      then echo "Now"
  elif [ $diff -lt 3600 ];    then echo "$(( diff / 60 ))min ago"
  elif [ $diff -lt 86400 ];   then echo "Today"
  elif [ $diff -lt 172800 ];  then echo "Yesterday"
  elif [ $diff -lt 604800 ];  then echo "$(( diff / 86400 )) days ago"
  elif [ $diff -lt 1209600 ]; then echo "1 week ago"
  elif [ $diff -lt 2592000 ]; then echo "$(( diff / 604800 )) weeks ago"
  elif [ $diff -lt 5184000 ]; then echo "1 month ago"
  elif [ $diff -lt 31536000 ]; then echo "$(( diff / 2592000 )) months ago"
  else echo "$(( diff / 31536000 )) years ago"
  fi
}
```

全履歴が一括操作のみで実質的な編集コミットが1件も見つからない場合（`effective_time` が空）は、生の `git log --follow -1` にフォールバックする（他に手がかりが無いための次善策。バッジは空欄にする＝安全側）。

### 4.6 バッジ記号（★/◎/△）

```bash
badge_for() {
  local f="$1"
  local t; t=$(effective_time "$f")
  [ -z "$t" ] && { echo ""; return; }
  local diff=$(( $(date +%s) - t ))
  if   [ $diff -lt 86400 ];  then echo "$badge_24h"
  elif [ $diff -lt 259200 ]; then echo "$badge_3d"
  elif [ $diff -lt 604800 ]; then echo "$badge_7d"
  else echo ""
  fi
}
```

（`$badge_24h` 等はセクション3で読み込んだ設定値、未設定時は `★`/`◎`/`△`）

---

## 5. テーブル別の適用手順

### 5.1 出張報告書テーブル（PUBLISH_SUMMARY.md基準）

各レポートフォルダの `PUBLISH_SUMMARY.md` は「実際に人が処理した日」を示す最も確実な指標である。git のコミット日時ではなく、これを使う。

```bash
today=$(date +%s)
for dir in Reports/*/; do
  f="${dir}PUBLISH_SUMMARY.md"
  [ -f "$f" ] || continue
  # head -1 必須：差分ビルドでは1行に日付が2つ入ることがあり、最新（先頭）のみ採用
  d=$(grep -A1 "## 実行日時" "$f" 2>/dev/null | tail -1 | grep -oE "[0-9]{4}-[0-9]{2}-[0-9]{2}" | head -1)
  [ -z "$d" ] && d=$(grep -oE "\*\*Date:\*\*[[:space:]]*[0-9-]+" "$f" 2>/dev/null | grep -oE "[0-9]{4}-[0-9]{2}-[0-9]{2}" | head -1)
  [ -z "$d" ] && continue
  t=$(date -j -f "%Y-%m-%d" "$d" +%s 2>/dev/null)
  [ -z "$t" ] && continue
  days=$(( (today - t) / 86400 ))
  echo "$dir : ${days}日前"
done
```

判定：

- 24時間以内 → `最終更新`列 `Today` / バッジ `★`
- 24時間超・3日以内 → `X days ago` / `◎`
- 3日超・7日以内 → `X days ago` / `△`
- 7日超・14日未満 → `1 week ago` / 空欄
- 14日以上 → `X weeks ago` / 空欄
- `PUBLISH_SUMMARY.md` が存在しない（このパイプラインを一度も通っていない） → `—` / 空欄

（`PUBLISH_SUMMARY.md` の日付は日単位のため、24時間判定の厳密な時刻比較ができない場合は「実行日が本日と同一なら★、それ以外は日数で判定」でよい。厳密にしたい場合は `YYYY-MM-DD HH:MM` のままUNIX秒に変換する。）

### 5.2 講演会レポート・Strategyテーブル（git履歴基準）

`Reports/講演フォルダ/*.md`・`strategy/*.md` はpublishパイプラインを通らないため `PUBLISH_SUMMARY.md` を持たない。セクション4の `find_real_last_edit()` / `effective_time()` / `rel_time()` / `badge_for()` をそのまま使う。

対象ファイルは、README.md の該当セクション（`## 講演会レポート`〜次の見出しの手前、`## Strategy`〜次の見出しの手前）に列挙されているリンク先を抽出して求める。

```bash
extract_section_links() {
  # $1: 見出し行の正規表現（例 "^## Strategy"）
  awk -v p="$1" '
    $0 ~ p {flag=1; next}
    flag && /^#{2,3} / {exit}
    flag
  ' README.md | grep -oE '\]\([^)]+\.md\)' | sed -E 's/^\]\(([^)]+)\)$/\1/'
}
```

### 5.3 知識ベース4テーブル（git履歴基準）

技術テーマ・企業情報・トレンド・アイデアの各サブセクション（`### 技術テーマ（Knowledge/）` 等）についても、`extract_section_links` で対象ファイルを抽出し、セクション4のロジックで `最終更新`・バッジを求める。

---

## 6. 書き換えルール（冪等性・安全性）

- 各テーブルの各行について、**先頭2列（`最終更新`・バッジ）だけ**を新しい値に置き換える。それ以外の列（日付・リンク・参加者・写真枚数・ナレッジ化・概要等）は一文字も変更しない。
- 行の追加・削除・並べ替えは行わない。README.md にまだ登録されていない新しいファイルがあっても、このスキルは行を追加しない（行の追加は make-report.md / archive-report.md の仕事）。
- テーブル・セクション自体が存在しない場合はスキップし、新規作成しない。
- 各テーブル直前の凡例キャプション行

  ```markdown
  > 最終更新列は **YYYY-MM-DD HH:MM** 時点の相対時間です（...）。★ = 過去24時間以内に実質的な更新があったもの　◎ = 過去3日以内（24時間超）に実質的な更新があったもの　△ = 過去7日以内（3日超）に実質的な更新があったもの
  ```

  の `**YYYY-MM-DD HH:MM**` 部分を、実行時刻（`date "+%Y-%m-%d %H:%M"`）に更新する。既存の括弧書き（「PUBLISH_SUMMARY.md の実行日時が基準」等の注記）はそのまま残す。凡例行自体が無い場合は追加しない（無理に体裁を変えない）。
- 同じ入力に対して再実行しても、値が変わらない行は変更しない（無駄な差分を作らない）。

---

## 7. 実行後の報告

作業完了後、簡潔に報告する。

```text
StarUpdate による README.md 表示更新を完了しました。

洗い直したテーブル：7（出張報告書・講演会レポート・Strategy・Knowledge・Companies・Trends・Ideas）
基準時刻：YYYY-MM-DD HH:MM

変化のあった行：
- 出張報告書：X行（例：202604-MODEX → Today / ★）
- 知識ベース（Companies）：X行
- ...（変化がなければ「変化なし」）
```
