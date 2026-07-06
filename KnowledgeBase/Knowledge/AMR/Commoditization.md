# AMR のコモディティ化

## 概要

2026年時点で AMR（Autonomous Mobile Robot）はコモディティ段階に入った。
中国・インド・韓国・米国・オランダ、多国籍プレイヤーが同一カテゴリで競合している。
日本の物流展で「だいたいわかっていた」認識は、MODEX を見て完全に覆された。

<img src="../../../Reports/202604-MODEX/Images/IMG_5736.JPG" width="800">
<p style="color:#888888; font-size:1.05em;">各社 AMR が複数台同時展示。ZS Robotics（中国）をはじめ、多国籍メーカーが同一フロアで競合を展開した（MODEX 2026）</p>

## MODEX 2026 での観察

- 棚搬送型 AMR・棚登り型（ASRS）・無人フォーク・パレット搬送・ヒューマノイドまで倉庫自動化の全段階が一覧できた
- 中国メーカーが来場者の真横で 5〜6m の高さにパレットを積み上げるデモ。安全性への絶対的自信がないとできない芸当
- インドメーカー（ADDVERB）は ABM コンセプトと同じフォーク式で世界展開済み
- MOODE ROBOT「No renovation」—既存の床・棚をそのまま使えることをウリにする

<br>
<p>
<img src="../../../Reports/202604-MODEX/Images/IMG_5728.JPG" width="390">
<img src="../../../Reports/202604-MODEX/Images/IMG_20260414_121814.jpg" width="390">
</p>
<p style="color:#888888; font-size:1.05em;">（左）ADDVERB（インド）。「True Human+Robot Collaboration」と「Pallet & Tote ASRS Solutions」の両輪で展開。Panasonic・Siemens との協業実績を掲示。（右）NewAge Industrial の AMR カート連動棚。棚搬送型 AMR が棚ごと自律搬送する（MODEX 2026）</p>

## 主要プレイヤー（MODEX 2026）

| 企業 | 国 | 特徴 |
|---|---|---|
| ZS Robotics | 中国 | シャトル型 ASRS、「0 PROJECT FAILURE RATE」 |
| HAI Robotics | 中国 | ACR（Autonomous Case-handling Robot）棚登り型 |
| ADDVERB | インド | フォーク型・人型ロボット協調、Panasonic/Siemens 協業 |
| MOODE ROBOT | 中国 | フォーク型 AMR、既存倉庫改修不要 |
| idealworks | ドイツ | パレットムーバー AMR、Zalando 導入実績 |
| SEER Robotics | 中国 | 全ロボット統合プラットフォーム |
| Locus Robotics | 米国 | 全工程ワンプラットフォーム（Case Picking〜Sanitation） |
| Tompkins + Duravant | 米国 | AMR ソーティングシステム |
| Smartpodd | スウェーデン系？ | フラットパレット搬送型 AMR |
| Multiway Robotics | 中国 | 大規模導入実績訴求 |
| Bluepath Robotics | 米国 | 「ROI in 2 Years」訴求 |
| Robotize | デンマーク | 超低床フラット AMR、パレット直搬送 |

<br>
<img src="../../../Reports/202604-MODEX/Images/IMG_20260415_113439.jpg" width="800">
<p style="color:#888888; font-size:1.05em;">Locus Robotics（米国）の大型ブース。「Case Picking / Returns / Transport / Sanitation」— 物流現場の全工程をワンプラットフォームで賄うコンセプト。AMR がコモディティ化した先にある「ソフトウェア統合」競争軸を体現していた（MODEX 2026）</p>

## ハノーバーメッセ 2026 での観察

<img src="../../../Reports/202604-HANNOVER/Images/IMG_20260420_154826.jpg" width="800">
<p style="color:#888888; font-size:1.05em;">会場通路を単独走行する小型AMR。来場者が行き交う通路を、監視員なしで人混みをかき分けながら進む。誰も驚かない。（ハノーバーメッセ 2026）</p>

- **最大の発見は「走っている場所」だった**：ブースを飛び出し、来場者が行き交う通路を1300kgを牽引しながら走行。監視員なし
- 誰も驚かず「当たり前の世の中になっている」（山崎 Nippou 3805）
- IntelブースではAMRの台車の上にヒューマノイドが立ち、AMR×ヒューマノイドの融合を実演
- MODEX 2026（アトランタ）と同一レベルかそれ以上の自信を現場で確認

## EP Equipment 社内での実稼働（2025年11月）

展示会のサンプルから一歩進んだ現実を見た。EP Equipment（浙江中力機械）の自社工場内で、AMR 150台が実際に稼働していた。

<br>
<img src="../../../Reports/202511-EP/Images/IMG_4064.JPG" width="800">
<p style="color:#888888; font-size:1.05em;">EP社自社工場内のAMRスタッカー。鉄製メッシュ箱型パレットをフリーロケーションで2段積みしている。床面の赤いレーザーは安全センサーの走査光。（EP Equipment 浙江工場 / 2025年11月26日）</p>

- 床に固定棚もレールもない。AMRが自分で場所を判断して段積みする「フリーロケーション」方式
- 鉄製箱型パレット（金属メッシュコンテナ）を2段積みで自律スタッキング
- AMR制御ユニットはUbuntuベース。ROS2との親和性が高いOSを採用している

<br>
<p>
<img src="../../../Reports/202511-EP/Images/IMG_4069.JPG" width="390">
<img src="../../../Reports/202511-EP/Images/IMG_4075.JPG" width="390">
</p>
<p style="color:#888888; font-size:1.05em;">（左）EP社AMR倉庫の通路。両側に積まれた鉄製パレットの間を、AMRが自律走行するための広い通路が確保されている。（右）EP社AMR制御ユニットのタッチパネル。Ubuntuのデスクトップが起動している。</p>

展示会のデモと実工場稼働の差は大きい。「150台が毎日止まらず動いている」という事実は、技術成熟度の証明だ。EP社開発者400名のうち200名がロボティクス担当。来年にはロボティクス専用開発センターが立ち上がる。

## 技術的示唆

- 差別化はもはや「AMR であること」ではなく「動作品質・信頼性・コスト」
- ソフトウェア統合（WMS・WCS 連携）が次の競争軸
- 「No renovation（既存設備改修なし）」コンセプトが中小規模顧客に響く

## スギヤス製品への応用可能性

- AMR と連携するリフト・テーブル製品の設計が次の要件に
- 「既存設備との統合」を前提にした製品設計が差別化になる可能性
- SEER Robotics（DMP 名義で接触済み）との連携検討価値あり

## 関連レポート

- [MODEX 2026 Report.md](../../../Reports/202604-MODEX/Report.md)
- [ハノーバーメッセ 2026 Report.md](../../../Reports/202604-HANNOVER/Report.md)
- [EP Equipment 工場訪問 2025年11月 Report.md](../../../Reports/202511-EP/Report.md)

## 更新履歴

| 日付 | 内容 |
|---|---|
| 2026-07-02 | MODEX 2026 から初期作成 |
| 2026-07-02 | ハノーバーメッセ 2026 の観察を追記 |
| 2026-07-03 | MODEX 写真を3枚追加（ADDVERB・NewAge AMRカート・Locus Robotics）|
| 2026-07-03 | EP Equipment 社内実稼働事例（150台・フリーロケーション段積み）を追記 |
