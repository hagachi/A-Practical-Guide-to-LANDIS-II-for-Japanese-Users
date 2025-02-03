# 3. Tips for data processing

## README first
- Model Introduction & Getting Started in Users tab (https://www.landis-ii.org/users)
- The LANDIS-II Exercise Book (https://www.landis-ii.org/users)
- Official training (https://www.landis-ii.org/training-and-conferences)

## Japanese specific tutorials

以下、順次日英併記に修正していきます。
主に、NEEを計算できるNECN-succession extentionを想定して書きます。

### 1. Species specific data / 植物種ごとのパラメータ
#### life history & functional traits
基本方針: 図鑑>論文>デフォルトの優先度ですべての植物種についてパラメータを整備します。

- Life history / 生活史
  - 主な情報源: 図鑑, 論文
- Functional traits / 形質
  - 主な情報源: 図鑑, 論文, CENTURY soil organic matter modelのデフォルト値

例: 
- 北海道厚岸町
  - 論文: https://www.frontiersin.org/journals/ecology-and-evolution/articles/10.3389/fevo.2020.00155/full
  - GitHub: https://github.com/hagachi/Project-RE-biodiversity-nexus-2020
- 北海道道南地域
  - 論文: https://www.sciencedirect.com/science/article/abs/pii/S030438002200179X
  - GitHub: https://github.com/hagachi/Project-LANDIS-Understory

#### parameters to be calibrated
基本方針: ANPP (地上部NPPの最大値(g-C month^-1)) や材の死亡率 (month^-1) など、感度が高く文献値が見つかりにくいパラメータは、ハイパーパラメータとして扱い、機械的にチューニングします。

- 芳賀がよく使う方法
  - 地域で使われる収穫予想表を教師データとする
    - モニタリングサイト1000のmortalityやフラックス研究で報告されているNPP最大値・LAI最大値などを制約にする
  - Spotpyでrandom sampling / LHSでパラメータを調整
  - 林野庁の森林生態系多様性基礎調査のバイオマス成長と比較して検証
- 課題
  - 収穫予想表が高齢林の成長を過小評価している件
    - 多様性基礎調査のバイオマス成長に合わせてチューニングする試みも実施中
  - 地域内での成長量の違いまでは再現できていなそうな件
    - 後述の土壌・気候の状態に応じて成長が変わるのを再現できるか・・・？

参考
- Haga et al. (2022) Ecol. Model. https://www.sciencedirect.com/science/article/abs/pii/S030438002200179X
- 片岡ら (2021) 環境情報科学 https://www.jstage.jst.go.jp/article/ceispapers/ceis35/0/ceis35_73/_article/-char/ja/


### 2. Environmental data
#### Vegetation
基本方針: 各グリッドセルに存在する植物コホートの初期状態を入力する。必要な情報は種名・コホートの齢・コホートの材積の3種類。出力結果に対する感度がとても大きい入力データ。

- 芳賀がよく使う方法
  - 森林簿から樹種名・樹齢(林齢で代用)・材積の情報を取得
  - 森林簿は広葉樹の情報が少ないので、広葉樹の種名は2.5万分の1植生図を参照
  - 欠損地は平均値で補完する
- 課題
  - 森林簿自体の精度問題 (もう少し森林のRSデータが揃ってきたら置き換えるのも手である)
    - 種名が合わない
    - 材積は実測ではなく、収穫予想表で推計された推計値なので実態と合わない

#### Soil
基本方針: 国内のデータセットを使う

- 「日本土壌インベントリー」. 参照: 2024 年 12 月 20 日. [Online]. 入手先: https://soil-inventory.rad.naro.go.jp/
- N. Yamashita ほか, 「National-scale 3D mapping of soil organic carbon in a Japanese forest considering microtopography and tephra deposition」, Geoderma , vol. 406, p. 115534, 1 月 2022, doi: 10.1016/j.geoderma.2021.115534.


#### Climate
基本方針: 現在の気象の平年値で対象地域をクラスタリングし、クラスタごとに将来気候データを入力する。

- 芳賀がよく使う方法
  - 平年値メッシュで対象地域をクラスタリング
  - クラスタ別に将来気候データを空間平均

- 国土交通省,「国土数値情報平年値メッシュ」. 2012年. [Online]. 入手先: https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-G02.html
- NIES2020-v1: https://www.nies.go.jp/doi/10.17595/20210501.001.html


