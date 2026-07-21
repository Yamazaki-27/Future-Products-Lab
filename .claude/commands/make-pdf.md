# make-pdf.md — Report.md 配布用PDF生成

完成した `Report.md` を、仲間・社内への配布用PDFに変換する。通常版（オリジナル画質）と軽量版（写真圧縮）の2種類を生成する。

## 発動条件

- 「PDFにして」「配布用にして」等の指示があったとき
- `review-report.md`・`publish-report.md` を経た Report.md を対象とすることが望ましい（初稿でも可）

## 前提

- macOS＋Google Chrome（ヘッドレスPDF印刷に使用。pandoc等は不要）
- 写真は `Core/make-report.md` のルールでリサイズ済み（横1600px程度）であること

## 手順

### STEP 1：画像参照プレフィックスの確認

Report.md 内の `<img src="...">` のパス（`../Images/`・`images/` 等）を確認する。以降のスクリプトの置換パターンをこれに合わせる。

### STEP 2：印刷用HTMLの生成

以下の変換スクリプトを一時フォルダに保存し、`SRC`（Report.mdのパス）と `DST`（出力HTML）を書き換えて実行する。

**レイアウトの要点（このCSS仕様を変えないこと）：**

- 写真＋キャプションは `.fig` ブロックで一体化し `break-inside: avoid`（ページ境界での泣き別れ禁止）
- キャプションは写真の直下4px・写真と同じ幅（縦位置写真でも左端が揃う）
- 縦位置写真（width="500"）はページ幅の62%、横位置（width="800"）は100%
- 横並びペア（width="390"）は flex で49%×2枚
- 横並びペアのキャプションが `<p>` ブロック内側に入っている場合も正しく拾う（`next_caption` 処理）
- A4・余白14mm/13mm・ヒラギノ・本文10.5pt
- Markdown表は罫線付きテーブルに変換する（`break-inside: avoid`／`---:` 指定列は右寄せ・折り返し禁止）。**この処理が無いと表が `| 項目 | ... |` の生テキストとしてPDFに出る**

```python
#!/usr/bin/env python3
"""Report.md → 印刷用HTML変換（写真＋キャプションを1ブロック化）"""
import re

SRC = "対象フォルダ/Report.md"      # 書き換える
DST = "対象フォルダ/Report_print.html"  # 書き換える

CSS = """
@page { size: A4; margin: 14mm 13mm; }
* { box-sizing: border-box; }
body { font-family: "Hiragino Sans", "Hiragino Kaku Gothic ProN", sans-serif;
       font-size: 10.5pt; line-height: 1.75; color: #222; margin: 0; }
h1 { font-size: 19pt; border-bottom: 3px solid #1E2761; padding-bottom: 6px; line-height: 1.4; }
h2 { font-size: 14.5pt; color: #1E2761; border-left: 6px solid #1E2761;
     padding-left: 8px; margin-top: 1.6em; break-after: avoid; }
h3 { font-size: 12pt; color: #1E2761; margin-top: 1.4em; break-after: avoid; }
p  { margin: 0.55em 0; }
ul { margin: 0.4em 0; padding-left: 1.5em; }
li { margin: 0.25em 0; }
blockquote { margin: 0.8em 0; padding: 0.4em 1em; border-left: 4px solid #CADCFC;
             background: #f5f8ff; color: #333; }
hr { border: none; border-top: 1px solid #bbb; margin: 1.6em 0; }
.fig { break-inside: avoid; page-break-inside: avoid; margin: 1.0em 0; }
.fig img { display: block; width: 100%; height: auto; border-radius: 3px; }
.fig.w800 { width: 100%; }
.fig.w500 { width: 62%; }
.fig .cap { color: #888888; font-size: 9pt; line-height: 1.5; margin: 4px 0 0 0; }
.imgpair { display: flex; gap: 2%; align-items: flex-start; }
.imgpair img { width: 49%; }
table { border-collapse: collapse; width: 100%; margin: 0.8em 0; font-size: 9.5pt;
        break-inside: avoid; page-break-inside: avoid; }
th, td { border: 1px solid #bbb; padding: 4px 8px; text-align: left; vertical-align: top; }
th { background: #eef2fb; color: #1E2761; font-weight: bold; }
td.num, th.num { text-align: right; white-space: nowrap; }
table td:first-child, table th:first-child { white-space: nowrap; }
"""

def inline(s):
    s = re.sub(r"\*\*(.+?)\*\*", r"<b>\1</b>", s)
    s = re.sub(r"\[([^\]]+)\]\(([^)]+)\)", r"\1", s)  # 配布PDFでは相対リンクをテキスト化
    return s

def cap_text(line):
    m = re.match(r"<p style[^>]*>(.*)</p>", line)
    return m.group(1) if m else line

def split_row(line):
    return [c.strip() for c in line.strip().strip("|").split("|")]

def is_sep(line):
    return bool(re.match(r"^\s*\|[\s:|-]+\|\s*$", line)) and "-" in line

lines = open(SRC, encoding="utf-8").read().splitlines()
out, i, n = [], 0, len(lines)
para, ul = [], []

def flush_para():
    global para
    if para:
        out.append("<p>" + "<br>\n".join(inline(x) for x in para) + "</p>")
        para = []

def flush_ul():
    global ul
    if ul:
        out.append("<ul>" + "\n".join("<li>%s</li>" % inline(x) for x in ul) + "</ul>")
        ul = []

def next_caption(idx):
    j = idx
    while j < n and (lines[j].strip() == "" or lines[j].startswith("<br>")):
        j += 1
    if j < n and lines[j].startswith("<p style"):
        return cap_text(lines[j].rstrip()), j
    return None, idx - 1

while i < n:
    line = lines[i].rstrip()
    # --- Markdown表（ヘッダ行＋区切り行＋データ行）。右寄せは `---:` で指定 ---
    if line.strip().startswith("|") and i + 1 < n and is_sep(lines[i + 1]):
        flush_para(); flush_ul()
        head = split_row(line)
        aligns = ["num" if c.strip().endswith(":") else "" for c in split_row(lines[i + 1])]
        rows = []
        i += 2
        while i < n and lines[i].strip().startswith("|"):
            rows.append(split_row(lines[i])); i += 1
        th = "".join('<th class="%s">%s</th>' % (aligns[k] if k < len(aligns) else "", inline(c))
                     for k, c in enumerate(head))
        body = ""
        for r in rows:
            body += "<tr>" + "".join('<td class="%s">%s</td>' % (aligns[k] if k < len(aligns) else "", inline(c))
                                     for k, c in enumerate(r)) + "</tr>"
        out.append("<table><thead><tr>%s</tr></thead><tbody>%s</tbody></table>" % (th, body))
        continue
    if line.startswith("<p>") and not line.startswith("<p style"):
        flush_para(); flush_ul()
        imgs = []
        i += 1
        while i < n and not lines[i].startswith("</p>"):
            if lines[i].strip():
                imgs.append(re.sub(r'\s*width="390"', "", lines[i].rstrip()))
            i += 1
        cap, j = next_caption(i + 1)
        block = '<div class="fig">\n<div class="imgpair">\n%s\n</div>' % "\n".join(imgs)
        if cap is not None:
            block += '\n<p class="cap">%s</p>' % cap
            i = j
        out.append(block + "\n</div>")
    elif line.startswith("<img"):
        flush_para(); flush_ul()
        w = "w500" if 'width="500"' in line else "w800"
        img = re.sub(r'\s*width="\d+"', "", line)
        cap, j = next_caption(i + 1)
        block = '<div class="fig %s">\n%s' % (w, img)
        if cap is not None:
            block += '\n<p class="cap">%s</p>' % cap
            i = j
        out.append(block + "\n</div>")
    elif line.startswith("<p style"):
        flush_para(); flush_ul()
        out.append('<p class="cap">%s</p>' % cap_text(line))
    elif line.startswith("<br>"):
        flush_para(); flush_ul()
    elif line.startswith("###"):
        flush_para(); flush_ul(); out.append("<h3>%s</h3>" % inline(line[3:].strip()))
    elif line.startswith("##"):
        flush_para(); flush_ul(); out.append("<h2>%s</h2>" % inline(line[2:].strip()))
    elif line.startswith("#"):
        flush_para(); flush_ul(); out.append("<h1>%s</h1>" % inline(line[1:].strip()))
    elif line.startswith(">"):
        flush_para(); flush_ul()
        out.append("<blockquote>%s</blockquote>" % inline(line[1:].strip()))
    elif re.match(r"^\s*- ", line):
        flush_para(); ul.append(re.sub(r"^\s*- ", "", line).strip())
    elif line.strip() == "---":
        flush_para(); flush_ul(); out.append("<hr>")
    elif line.strip() == "":
        flush_para(); flush_ul()
    else:
        flush_ul()
        para.append(re.sub(r"<br>$", "", line))
    i += 1
flush_para(); flush_ul()

doc = ("<!DOCTYPE html><html lang='ja'><head><meta charset='utf-8'>"
       "<title>タイトル（Report.mdのH1に合わせる）</title>"
       "<style>%s</style></head><body>\n%s\n</body></html>") % (CSS, "\n".join(out))
open(DST, "w", encoding="utf-8").write(doc)
print("HTML written:", DST)
```

### STEP 3：通常版PDFの生成（Chromeヘッドレス）

```bash
CHROME="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$CHROME" --headless --disable-gpu --no-pdf-header-footer --virtual-time-budget=20000 \
  --print-to-pdf="対象フォルダ/レポート名_YYYYMM.pdf" \
  "file://対象フォルダ/Report_print.html"
```

- パスは必ず**絶対パス**で指定する（カレントディレクトリはセッション間で保証されない）

### STEP 4：軽量版PDFの生成

写真を圧縮したミラーを作り、HTMLの参照先を差し替えて再度PDF化する。

```bash
# 1) 圧縮ミラー作成（500px・JPEG品質40。unUsedは除外）
MIRROR="一時フォルダ/Images_light"
mkdir -p "$MIRROR/OtherPictures"
find "画像フォルダ" -maxdepth 2 -type f \( -iname "*.jpg" -o -iname "*.jpeg" \) ! -path "*/unUsed/*" | while IFS= read -r f; do
  rel="${f#画像フォルダ/}"
  sips -s format jpeg -s formatOptions 40 -Z 500 "$f" --out "$MIRROR/$rel" >/dev/null 2>&1
done

# 2) 参照差し替え → PDF化
sed "s|\.\./Images/|$MIRROR/|g" Report_print.html > Report_print_light.html
"$CHROME" --headless --disable-gpu --no-pdf-header-footer --virtual-time-budget=20000 \
  --print-to-pdf="対象フォルダ/レポート名_YYYYMM_軽量版.pdf" "file://.../Report_print_light.html"
```

- **注意：Chromeは印刷時に画像を再エンコードするため、JPEG品質を下げるだけではPDFは小さくならない。ピクセルサイズ（-Z 500）を落とすことが効く**

### STEP 5：検証と後始末

1. `qlmanage -t -s 1200 -o 一時フォルダ/ 生成した.pdf` で1ページ目をサムネイル化し、Read ツールで目視確認する（タイトル・写真とキャプションの密着・箇条書きの体裁）
2. 中間ファイル（Report_print.html・ミラー）を削除する
3. 生成したPDFのファイル名・サイズを CHANGELOG.md に記録する

## 重要ルール

- **PDFは git にコミットしない**（数十MBになりリポジトリが肥大化する。配布は AirDrop・LINE・チャットで行う）。リポジトリ直下の `.gitignore` に `*.pdf` を設定済み（2026-07-21）。コミットが必要な場合はユーザーの明示的な了解を得て `.gitignore` を調整する
- 1ファイル100MB超はGitHubにpush不可
- Report.md 本文には一切手を入れない（PDF化は表示変換のみ）
- 再生成は何度でも安全に行える（冪等）。本文修正のたびに STEP 2〜5 を再実行する
