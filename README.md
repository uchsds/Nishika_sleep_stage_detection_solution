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
・EEG Fpz-Cz, EEG Pz-Oz, EOG Pz-Oz, Resp oro-nasal, EMG submental, Temp rectalを標準化した後の30秒間(1epoch)の平均値, 標準偏差<br>
・EEG Fpz-Cz, EEG Pz-Ozの以下の周波数領域のパワースペクトル密度<br>
　　デルタ波 　　　　　：0.4～4 Hz<br>
　　シータ波　　　　　：4～8 Hz<br>
　　スローアルファ波　：8～9 Hz<br>
　　ミッドアルファ波　：9～12 Hz<br>
　　ファストアルファ波：12～14 Hz<br>
　　ベータ波　　　　　：14～26 Hz<br>
　　ガンマ波Low　　　：30～50 Hz<br>
　　ガンマ波Medium　：60～80 Hz　 →この領域は存在しないので最終的にはdrop<br>
　　ガンマ波High　　　：90～110 Hz　→この領域は存在しないので最終的にはdrop<br>
・EOG horizontalも上の周波数領域を利用(細かく分割されているので)<br>
・EEG Fpz-Cz, EEG Pz-Oz, EOG horizontalの3種類のアルファ波パワースペクトル密度の合計値<br>
・ここまでに作成した特徴量の2区間のlag, ma, max, min, deruta<br>
・測定日(1日目or2日目)<br>
・年齢<br>
・性別<br>
・消灯してからの時間<br>
<br>
●データの分割<br>
・StratifiedGroup 5-Fold (groupはid)<br>
<br>
●モデル<br>
・5つのデータセットに対してLightGBMモデル作成<br>
・ハイパーパラメーターはOptunaで最適化<br>
・特徴量重要度の1例を下記に示しますが、消灯してからの経過時間(off_time)の効果が大きかったです。<br>
![image](https://user-images.githubusercontent.com/118031932/213706911-f3662ea0-777d-402e-9163-1740cc49e737.png)
<br>
●後処理<br>
・一つ前と一つ後の判定が同じときは間の判定状態を1つ前の判定に揃える<br>
・覚醒からレムに移ることはないので、覚醒→レムのときレムの判定は覚醒に<br>
<br>
●最終予測<br>
・5モデルの予測値の多数決<br>
<br>
●予測結果<br>
・CV：0.852919, 0.872428, 0.866832, 0.862979, 0.833410 <br>
・暫定：0.819825<br>
・最終：0.830471<br>
<br>
<br>
<br>
# Lisence
<br>
This project is licensed under the MIT License, see the LICENSE file for details
