# R4（2020）年度キチジ太平洋北部の資源評価  
---
## 必要なデータセット
●は昨年のデータをそのまま使い回すもの，○は昨年のデータに修正が必要なものを表す     

#### 調査データ
○ ALdata.xlsx: 2019年以降の年齢査定結果．最新年を更新  
○ survey_N_at_length: 前年10月のトロール調査で得られた体長別資源尾数（mdbより．魚種別体長別資源量のNS別のデータ．q_魚種別体長別資源量2021.xlsx）  
○ trawl_N_at_length_ns.csv: トロール調査で得られた南北別体長別資源尾数（mdbより）．最新年分を更新   
○ trawl_ns_length2.csv: 2021年のトロール調査で得られた南北別体長別資源尾数詳細（mdbより）  
○ 操業記録データ2021.xlsx: 2021年のトロール調査点の緯度経度情報（mdbより）  
○ q_魚種別体長別資源量2021.xlsx: 2021年のトロール調査結果（mdbより）  
● survey_N_at_age.csv: 2018年までの調査で得られた年齢別資源尾数   
● olddata_trawl.csv: 2018年までのトロール調査で得られた年齢別漁獲量と年齢別漁獲尾数，および（沖底）漁獲量をまとめたもの（過去のエクセルより）  

#### 漁獲量データ（沖底）
○ okisoko_after2019.xlsx: 2019年以降の沖底データ（mdbより）．最新年を更新    
● okisoko_old.csv: 2018年までの沖底データ（mdbより）    

#### 漁獲量データ（県）
○ catch_pref.xlsx: 県からもらった漁獲量データ．古いフォーマット（.xls）を読み込めるコードにはしていないので，名前をつけて保存でxlsxにする  
● catchdata_old.csv: 2018年までの県からもらった漁獲量データをcsvにしたもの  
● effortdata_old.csv: 2018年までの県からもらった努力量データをcsvにしたもの  
○ catch_miyagi.csv: 宮城県からもらった2021年の漁獲物データ．catch_pref.csvを作成したデータの別シート    
○ taityo_miyagi_fresco.csv: 宮城県からもらった2021年の漁獲物の体長データ（その1）．フレスコから落とす  
○ yatyo_miyagi.csv: 宮城県からもらった2021年の漁獲物の体長データ（その2）．県からもらった箱入れキチジ体長.xlsxをcsv化  
○ hati_sokutei.csv: 青森県からもらった2021年の漁獲物の体長データ．漁獲量データと同じだけど，銘柄を修正する必要がある  
○ hati_sokutei2019.csv: 青森県からもらった2019年の漁獲物の体長データ．規格名と規格コードのデータフレームを作成するためだけに使用　　
○ taityo_FRA.csv: 八戸庁舎で精密測定した2021年の体長データ（sokouoより）  


#### その他   
● rawdata_fig5_for_nextSA.csv: 漁獲量の時系列データ（昨年度の資源評価より）  
● Map.txt: 調査域の緯度経度情報（由来不明）  
● キアンコウ緯度経度.txt: 海区の緯度経度（由来不明）   
● marmap_coord.csv: 等深線の情報（由来不明）    

 

---
## Rmarkdownの構成
チャンク1-12では，資源量の計算やドキュメントに必要な図表の保存を行う部分．チャンク13以降はドキュメントの修正箇所をHTMLで表示する部分．  
以下はチャンク1-12の詳細．  

### 1. ```{r prefecture}```
県からもらった漁獲量の処理    
!!県の漁獲量を資源量計算に使用するため，資源量計算の前に処理する必要がある!!        
    
- step1. 県から提出された漁獲量データ（沖底以外）を集計する    
!!県から提出されたデータの構造に合わせてコードの修正が必要!!    
県が提出する「沖底」の漁獲量 ≠ 沖底漁績の値，なので，step2で修正する    
            
        
- step2. 漁獲量データを集計する    
-- step2-0. 昨年度の図5のデータ&最新版の沖底漁績データの読み込み    
-- step2-1. 沖底漁績の確定値をstep.2-0に入れる    
-- step2-2. 沖底漁績の暫定値をstep.1の県データに入れる    
            県が提出する「沖底」の漁獲量 ≠ 沖底漁績の値，に注意    
-- step2-3. 図5に必要なデータの統合    
-- step2-4. 表1のデータを作成    
     
- step3. 作図    


### 2. ```{r get biomass}```
資源量の計算   
・10月の調査で得られた体長別漁獲尾数（面積密度法で引き延ばし済み）にALKをかけて年齢別漁獲尾数に変換し，さらに調査での採集効率が年齢で変わる仮定をかけて10月の年齢別資源尾数を算出（step1-5部分）   
・1月の資源量を知りたいので，調査後2ヶ月分の死亡率をかける（step7-8）   
- step1. Number at ageデータの作成    
-- step1-1. Age-Length Keyの作成    
-- step1-2. von Bertalanffy growth curveの推定    
-- step1-3. 年齢別体長別の尾数データの算出（これは資源評価結果ではない）    
-- step1-4. 年齢別の平均体長と平均重量を算出    
    
- step2. 年齢別の生存率を算出（1->2歳魚の生残率のみ，翌年の資源量計算で使う）
     
- step3. 体長別の採集効率(q)を考える
        
- step4. 体長から体重を算出
            
- step5. 採集効率が年齢で変わることを考慮して10月（調査時）の資源量と資源尾数を算出

- step6. 年齢別の生存率を算出（3歳魚以上の生残率のみ，翌年の資源量計算で使う）
            
- step7. 1月時点での資源量を算出するため，調査が行われた10月以降の2ヶ月分の漁獲率や自然死亡率などを仮定する
            
- step8. 10月の調査データから1月の資源量を算出
        
- step9. 漁獲割合
            
- step10. 作図; figs. 10-12



### 3. ```{r get srr}```
再生産関係の計算
- step1. 調査で得られた資源尾数（南北海域別）
    
- step2. 再生産関係（SRR）
 
- step3. 作図; 図13と15

### 4. ```{r ABC}```
ABCの計算  
- 直近3年のFの平均値をF currentとする　　
- 翌年の資源量を計算（1歳魚の生残率 [チャンク get biomassのstep2で計算] とそれ以降の年齢の生残率を仮定）　　


### 5. ```{r fig9}```
青森県と宮城県における漁獲物の体長組成の作成    
**R3年度まで: 宮城県の体長組成を宮城県+福島県+茨城県の漁獲量で引き延ばし，青森県の体長組成を青森県+岩手県の漁獲量で引き延ばし**    
**R4年度: 青森県と宮城県のみの体長組成を頻度グラフとして提出（つまり漁獲量で引き伸ばさない）**    
- step1. 各県の漁獲量データ
    
- step2. 宮城県の体長組成
- step3. 青森県の体長組成
- step4. 作図; 図9



### 6. ```{r okisoko}```
沖底漁績データの集計


### 7. ```{r figA3-1}```
補足図3-1の作成


### 8. ```{r figA3-2}```
補足図3-2の作成


### 9. ```{r figA3-4}```
補足図3-4の作成


### 10. ```{r fig4}```
図4の作成


### 11. ```{r figsA3}```
補足図3の作成


### 12. ```{r save}```
ディレクトリの下にoutputsフォルダを作成し，ドキュメントに必要な図表を出す   

[メモ]   
表は表.xlsxで取りまとめて，ドキュメント作成とその後のデータ提出に使用   
※ R3年度: 表を全て縦向きにするよう指示があったため，昔の形式（横向きの表）は全て表.xlsxから削除   
※ R4年度: 表の体裁を修正．表の注釈を毎年修正しなくて済むように変更   

---
## 注意点   
#### mdb由来のデータ関連
ggplot2を用いた作図部分にはMac固有のコード（日本語表記に関するもの）が含まれているため，windowsではエラーが出る可能性がある．適宜削除が必要．

#### tidyverse関連
公開コードは主担当者のPC環境では問題なく結果が出ることを確認しているが，tidyverseを使う部分で不具合が出ることもあるよう．    
以下，主担当者のPCで不具合が出やすい関数とコード修正の例     
・mutate(): $使って書き換える  
・group_by(), summarize(): name spaceを入れる  
・rename(): name spaceを入れる．それでもダメならcolnames()   

#### mdb由来のデータ関連
mdbからエクスポートされたエクセルデータは，そのまま読み込むと列名を認識してくれずデータの途中から読み込まれる時がある．（読み込みオプションなど色々試したが）手元のPCで名前をつけて保存を使って上書き保存するのが一番手っ取り早い    

---
## 
