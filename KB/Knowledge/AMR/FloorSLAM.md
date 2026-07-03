# Floor SLAM ― 床面認識による誘導方式

## 概要

四恩システム（久留米・40名）が製品化した、床面の傷・汚れ・凹凸を特徴点として自己位置推定を行う AMR 誘導方式。
磁気テープや QR コードを必要とせず、走行しながらマップを更新し続ける。
ヨーロッパ発の技術を国内中小メーカーが製品化した点で、市場ギャップを示す事例でもある。

<br>
<img src="../../202606-InnovationEXPO/images/四恩3.JPG" width="800">
<p style="color:#888888; font-size:1.05em;">四恩システムのAGV全景とFloor SLAM技術パネル。ガイド不要、ルート変更即対応が特徴。（INNOVATION EXPO 2026）</p>

## 技術特徴

<br>
<p>
<img src="../../202606-InnovationEXPO/images/yamazaki/IMG_7624.JPG" width="390">
<img src="../../202606-InnovationEXPO/images/yamazaki/IMG_7625.JPG" width="390">
</p>
<p style="color:#888888; font-size:1.05em;">四恩システム AGV 実機展示。スバルに約30台導入済み。（INNOVATION EXPO 2026）</p>

- 床面の傷・模様・凹凸を「特徴点」として認識・記憶
- 走行するたびに床情報を更新し、傷や汚れなどの環境変化に追従
- 磁気テープ・QRコード・インフラ改修が不要
- ルート変更がソフトウェアで即対応可能
- **スバルへ約30台導入済み**（2026年時点）

## INNOVATION EXPO 2026 での観察

- 九州・久留米の40名規模メーカー「四恩システム」が出展
- 創業15年・44歳の社長。「AMRをやるつもりは当初まったくなかった」
- 東京にも活動拠点。山崎部長と再面談を約束
- 前川TLの視点：「現在のABMシリーズへの複数誘導方式対応は提案の幅を広げる可能性」

## 誘導方式比較

| 方式 | 要インフラ | 変更柔軟性 | 主な用途 |
|---|---|---|---|
| 磁気テープ | 床テープ貼り付け | 低（物理変更必要）| 工場・倉庫の定型ルート |
| QRコード | QRシール貼り付け | 中 | 棚搬送・WMS連携 |
| **Floor SLAM** | **なし** | **高（ソフトのみ）** | **旧設備・農場・古い工場** |
| LiDAR SLAM | なし | 高 | 汎用 AMR |

## スギヤスへの示唆

- ABMシリーズの誘導方式をビニールテープ・QRコード・Floor SLAM から選択できる構成は顧客ニーズに直結する
- 農場・古い工場・倉庫リフォーム後の現場など、インフラ改修ができない環境への対応力が増す
- 四恩システムとの**技術提携・OEM・ライセンス**は検討価値がある

## 関連

- [Companies/四恩システム.md](../../Companies/四恩システム.md)
- [Ideas/FloorSLAM_ABM_NavigationOption.md](../../Ideas/FloorSLAM_ABM_NavigationOption.md)
- [INNOVATION EXPO 2026 Report.md](../../202606-InnovationEXPO/Report.md)

## 更新履歴

| 日付 | 内容 |
|---|---|
| 2026-07-03 | INNOVATION EXPO 2026 から初期作成 |
