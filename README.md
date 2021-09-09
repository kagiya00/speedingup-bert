# speeding up bert

自然言語処理の bert 系機械学習モデルに対して、推論を高速化する手法の比較評価実験の結果です。利用したコードも公開しています。



## 評価結果

以下の手法を評価しました。

- 量子化： ニューラルネットワークのパラメータを浮動小数点型から整数型に変更する
- MAX_LENGTHの縮小： モデルに入力する文の語数の上限値（MAX_LENGTH) を下げる

結果は以下の通りです。CPUを用いたベースモデル(base_CPU)に対し、量子化とMAX_LENGTHの縮小を行ったモデル(q_ml128, q_ml064)では、精度は低下するものの（94%→84%～75%）、性能は５～８倍になりました。

<img src="images\chart1_result.jpg" alt="図１ 評価結果" style="zoom:100%;" />

<img src="images\table1_result.jpg" alt="表1 評価結果" style="zoom:100%;" />



## 評価条件

- データセット： [livedoor ニュース](https://www.rondhuit.com/download.html#ldcc)

<img src="images\table2_training_data.jpg" alt="表2 学習データ" style="zoom:100%;" />



<img src="images\table3_test_data.jpg" alt="表3 テストデータ" style="zoom:100%;" />

- 学習モデル： [cl-tohoku/bert-base-japanese-whole-word-masking](https://huggingface.co/cl-tohoku/bert-base-japanese-whole-word-masking)



## コードの実行方法

Google Colaboratory に以下のコードをアップして、上から順番に実行してください。  
1.のコードの出力ファイルを2.のコードで読み込み、2.のコードの出力ファイルを3.以降のコードで読み込んでいます。前のコードをシャットダウンしておかないと、後のコードでファイルの読み込みに失敗するかもしれません。

1.  src/1_news_cl_pre.ipynb  ： データセットの読み込みと前処理
2.  src/2_news_cl_train.ipynb ： 事前学習済みモデルの読み込みとファインチューニング。
3.  src/3_news_cl_test_base_model_with_GPU.ipynb ： base_GPUモデルの評価
4.  src/4_news_cl_test_base_model_with_CPU.ipynb ： base_CPUモデルの評価
5.  src/5_news_cl_test_quantization_ML512_with_CPU.ipynb ： q_ml512モデルの評価
6.  src/6_news_cl_test_quantization_ML256_with_CPU.ipynb ： q_ml256モデルの評価
7.  src/7_news_cl_test_quantization_ML128_with_CPU.ipynb ： q_ml128モデルの評価
8.  src/8_news_cl_test_quantization_ML064_with_CPU.ipynb ： q_ml064モデルの評価

Google Colaboratory Pro にて動作確認しています。





## リファレンス

本サンプルコードの作成にあたり、以下を参考にさせていただきました。どうもありがとうございます。

- [BERTの推論速度を最大10倍にしてデプロイした話とそのTips](https://tech.jxpress.net/entry/2021/08/26/170000)
- [BERTによる自然言語処理を学ぼう！ -Attention、TransformerからBERTへとつながるNLP技術-](https://github.com/yukinaga/bert_nlp)
- [Python自然言語処理101本ノック:: ～基礎からBERTまで～](https://www.amazon.co.jp/dp/B08DMJQHHL)

