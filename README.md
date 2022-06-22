# R4（2020）年度キチジ太平洋北部の資源評価
## 必要なデータセット
#### （●は昨年のデータをそのまま使い回すもの，○は昨年のデータに修正が必要なものを表す）    
####  R2の『引き継ぎ資料2 (chap2.R) 』部分のデータセット
○ ALdata.xlsx: 年齢査定したデータ．パートさんからもらったファイルに年齢査定結果を手入力  
○ survey_N_at_length: 前年10月のトロール調査で得られた体長別資源尾数（mdbより．魚種別体長別資源量のNS別のデータ．q_魚種別体長別資源量2021.xlsx）  
○ okisoko_after2019.xlsx: 2019年以降の沖底データ（mdbより）    
● okisoko_old.csv: 2018年までの沖底データ（mdbより）    
○ catch_pref.xlsx: 県からもらった漁獲量データ．古いフォーマット（.xls）を読み込めるコードにはしていないので，名前をつけて保存でxlsxにする  
● catchdata_old.csv: 2018年までの県からもらった漁獲量データをcsvにしたもの  
● effortdata_old.csv: 2018年までの県からもらった努力量データをcsvにしたもの  
● olddata_trawl.csv: 2018年までのトロール調査で得られた年齢別漁獲量と年齢別漁獲尾数，および（沖底）漁獲量をまとめたもの（過去のエクセルより）  
○ trawl_N_at_length_ns.csv: 2019年までのトロール調査で得られた南北別体長別資源尾数（mdbより）        　　
○ rawdata_fig5_for_nextSA.csv: 漁獲量の時系列データ（昨年度の資源評価より）  

#### R2の『引き継ぎ資料3 (chap3.R) 』部分のデータセット
●Map.txt: 調査域の緯度経度情報（由来不明）  
●キアンコウ緯度経度.txt: 海区の緯度経度（由来不明）  
○catch_miyagi.csv: 宮城県からもらった2021年の漁獲物データ．catch_pref.csvを作成したデータの別シート    
○taityo_miyagi_fresco.csv: 宮城県からもらった2021年の漁獲物の体長データ（その1）．フレスコから落とす  
○yatyo_miyagi.csv: 宮城県からもらった2021年の漁獲物の体長データ（その2）．県からもらった箱入れキチジ体長.xlsxをcsv化  
○hati_sokutei.csv: 青森県からもらった2021年の漁獲物の体長データ．漁獲量データと同じだけど，銘柄を修正する必要がある  
○hati_sokutei2019.csv: 青森県からもらった2019年の漁獲物の体長データ．規格名と規格コードのデータフレームを作成するためだけに使用　　
○taityo_FRA.csv: 八戸庁舎で精密測定した2021年の体長データ（sokouoより）  
○trawl_ns_length2.csv: 2021年のトロール調査で得られた南北別体長別資源尾数詳細（mdbより）  
○lonlat2021.csv: 2021年のトロール調査点の緯度経度情報（mdbより）  
●marmap_coord.csv: 等深線の情報（由来不明）  
 
## Rmarkdownでよく出る不具合
tidyverseを使う部分で不具合が出ることが多い．原因不明なので，name spaceを入れたり別の関数を使ったりして，個別のマシンで動くように書き換える．なお，主担当者のPC環境では問題なく結果が出ることを確認済み  
例）  
・mutate()を使って新しい列を入れる: $使って書き換える  
・group_by()とsummarize(): name spaceを入れる  
