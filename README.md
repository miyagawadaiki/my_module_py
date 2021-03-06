# simu_file_handler

## 概要
このモジュールは主にシミュレーションプログラムにおける使用を想定している．

### ユースケース
特に次のような場合において役立つだろう．
* 多くのパラメータがあり，それらによるパラメータ空間内のあらゆる組み合わせを試す必要があるとき（パラメータスタディ）
* 乱数を用いたシミュレーション（モンテカルロ法）のため，複数回の試行によって平均化した値を使用する必要があるとき

### 行えること
上記の場合において，このモジュールは次の処理を簡単にコーディングできるようサポートする．
* パラメータセットごとにファイルを生成し，試行ごとの結果値を記録する
* パラメータセットごとのファイルから```n```回試行平均の値を得る
* 【発展】とあるパラメータを```start```から```goal```まで刻み幅```sep```で動かしたときの，それぞれでの```n```回試行平均値の配列を得る
* 【発展】2つのパラメータを動かしたときの```n```回試行平均値の行列を得る（そのままmatplotlib.pyplot.imshowなどに渡せる）

<br>

## 使用方法
一度使ってみたいなら[Sample/BinDist.ipynb](https://github.com/miyagawadaiki/simu_file_handler/blob/master/Sample/BinDist.ipynb)か[Sample/EvolGame.ipynb](https://github.com/miyagawadaiki/simu_file_handler/blob/master/Sample/EvolGame.ipynb)を使ってみることを推奨する(それぞれ二項分布，進化ゲームのシミュレーションに適用してみた例である)．より詳しくは[reference.md](https://github.com/miyagawadaiki/simu_file_handler/blob/master/reference.md)に記述してある．

### 1. importする
例えば，下記のようにタイプしてまずモジュールをimportする（これはソースコードと同じ場所にsimu_file_handler.pyがある場合の書き方）
```python
sys.path.append("./")
import simu_file_handler as sh
```

### 2. 変数のクラスを定義する（SParameterクラスを継承する）
シミュレーションで使用される変数をSParameterクラスを継承したクラスによってまとめる．  

例えば，変数a,b,c,dを用いるならば，次のように書ける．

```python
class Parameter(sh.SParameter):
    
    # パラメータの設定
    def __init__(self, a=1.0, b=0.01, c=0.01, d=1.0, sd=None):
        # 親クラスの初期化
        super().__init__(sd)
        
        self.pdict['a'] = a
        self.pdict['b'] = b
        self.pdict['c'] = c
        self.pdict['d'] = 
```

### 3. SimuFileHandlerクラスのインスタンスを生成する
データ格納用メソッドなどを持った`SimuFindHandler`クラスのインスタンスを作る．  
その際，データ格納用のフォルダパスを必要とするので，先に用意しておく必要がある．

例えば，以下のように書く．

```python
folder = './data'
sfh = sh.SimuFileHandler(folder, Parameter())
```

### 4. シミュレーションコードを書き，データを追加する
シミュレーションコードは各々が目的に従ったものを書けば良い．  

データをフォルダに追加するためには`add_one_result`メソッドや`add_results`メソッドを使う必要がある．

### 5. データを読み出す
プロット等でデータを読み込みたい場合は，`get_and_read_ave`メソッドや，変数の範囲を指定して読み込める`get_ave_1D`メソッドを用いることができる．

### 番外編. 格納したデータのサマリ
`SimuFileHandler.summary`メソッドにより，どの変数を重点的に調べたかなどを視覚的に表示できる（ある程度は見れるがまだ開発中 06/17）
