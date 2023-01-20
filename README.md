# 【Nishika】睡眠段階の判定 〜”睡眠の深さを判別しよう”〜【20th_place_solution】

## 目次<br>
[コンペ概要](#コンペ概要)<br>
[結果](#結果)<br>
[解法](#解法)<br>
<br>
## コンペ概要<br>
コンペサイト：https://www.nishika.com/competitions/sleep/summary<br>
<br>
睡眠ポリグラフ(PSG)から睡眠の深さ(睡眠段階)を予測<br>
評価指標はAccuracy<br>
<br>
<br>
## 結果<br>
暫定：23位(スコア：0.819825)<br>
最終：20位(スコア：0.830471)<br>
<br>
<br>
## 解法<br>
●特徴量作成<br>
・EEG Fpz-Cz, EEG Pz-Oz, EOG Pz-Oz, Resp oro-nasal, EMG submental, Temp rectalの30秒間(1epoch)の平均値, 標準偏差, epoch平均-全体平均, epoch標準偏差-全体標準偏差<br>
・EEG Fpz-Cz, EEG Pz-Oz, EOG Pz-Oz, Resp oro-nasal, EMG submental, Temp rectalの30秒間(1epoch)の平均値, 標準偏差<br>
・EEG Fpz-Cz, EEG Pz-Ozの以下の周波数領域のパワースペクトル密度を取得<br>
　　デルタ波 　　　　　：0.4～4 Hz<br>
　　シータ波　　　　　：4～8 Hz<br>
　　スローアルファ波　：8～9 Hz<br>
　　ミッドアルファ波　：9～12 Hz<br>
　　ファストアルファ波：12～14 Hz<br>
　　ベータ波　　　　　：14～26 Hz<br>
　　ガンマ波Low　　　：30～50 Hz<br>
　　ガンマ波Medium　：60～80 Hz　 →この領域はNaNなので最終的にはdrop<br>
　　ガンマ波High　　　：90～110 Hz　→この領域はNaNなので最終的にはdrop<br>
・EOG horizontalも上の周波数領域を利用(細かく分割されているので)<br>
・EEG Fpz-Cz, EEG Pz-Oz, EOG horizontalの3種類のアルファ波パワースペクトル密度の合計値<br>
・ここまでに作成した特徴量の2区間のlag, ma, max, min, deruta<br>
・治験日(1日目or2日目)<br>
・年齢<br>
・性別<br>
・消灯してからの時間<br>
<br>
●データの分割<br>
・StratifiedGroup 5-Fold<br>
<br>
●モデル<br>
・5つのデータセットに対してLightGBMモデル作成<br>
・ハイパーパラメーターはOptunaで最適化<br>
<br>
●最終予測<br>
・5モデルの予測値の多数決<br>
●提出ファイル<br>
①：上記のモデル(暫定：0.819825, 最終：0.830471)<br>
②：上記のモデルでlagなどの区間を2→1にしたもの(暫定：0.816517, 最終：0.827386)<br>
<br>
<br>
<br>
# Lisence
<br>
This project is licensed under the MIT License, see the LICENSE file for details
