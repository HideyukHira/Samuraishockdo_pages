---
title: Windows10 のAnaconda環境でTensorflowのGPU版を使う方法
---
# Windows10 のAnaconda環境でTensorflowのGPU版を使う方法
Windows10 のAnaconda環境で&nbsp;TensorflowのGPU版。  
環境構築に何回か手間どりました、メモがてら共有させていただきます。  
Anaconda環境はじめ、もろもろの具体的なインストール方法やPATHの設定など細かいところは検索すると解決すると思いますので、割愛させていただきました。  
インストールの順番、ソフトウェアなどの正しいバージョンの組み合わせを知る方法の参考としてご覧ください。  
## TensorflowのGPU版ハードウェア環境１
* OS windows10  
* PC　HP Z800
* GPU NVIDIA QUADRO K600
## TensorflowのGPU版ハードウェア環境2
* OS WINDOWS10
* PC　DELL OptiPlex 7080
* GPU NVIDIA GeForce GTX 1660 SUPER
## GPUを認識できたTensorFlow 関連のソフトウェアバージョン
私が使用しているWINDOWSパソコンの上記のハードウェア環境１と２両方共通で動いた（GPU認識できた）組み合わせです。  
Anaconda環境でのみ確認しています。  
### Anaconda 環境
* Python3.7
* CUDA TOOLKIT 10.1
* Visual studio 2019
* cuDNN 7.6
* tensorflow-2.1.0 (バージョン指定でGPU有でpython3.7に合わせる）
### インストール順・考え方など
GPUを利用するには、GPUボード（通常はグラフィックカード）が物理的に必須になります。 
ソフトウェア面は、インストールやり直しとかできますが、グラボをとっかえひっかえするわけにはいかないですよね。 
なので、GPUから始めることにします。    
最初にAnacondaをインストール  
いわゆるAnaconda環境をインストールします。  
baseのみで大丈夫です、新たな環境などは後程インストールします。  
NVIDIAのGPUドライバの最新版をインストール  
最新版をダウンロードインストールします。  
ダウンロード画面に  
バージョン: R450 U5 (452.57) WHQL　などの表記があるので、452.57　部分をメモしておきます。  
ダウンロードインストールします。  
NVIDIAのCUDA TOOLKITの適切なバージョンを確認します  
このページを見ます[https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)  
CUDA Driver　の表を見ます。上記のGPUのドライバのバージョンだと452.57ですので  
CUDA 11.0.3 Update 1 &gt;= 450.51.06 &gt;= 451.82  
に当てはまることになり、CUDA 11.0.3 Update 1　以下であれば大丈夫なんだなと認識します。  
ダウンロードはまだしません。対象バージョン「以下」って考え方がが大切かなと感じました。  
ここで、自分のハードウェア環境とTensorflowとの相性を判定  
TensorflowでGPUを認識させるには、  
* GPUのバージョン
* CUDAのバージョン
* cuDNNのバージョン
* Pythonのバージョン
が事前に正しい組み合わせであることが必要です。  
この４つを正しい組み合わせでインストール  
その組み合わせにマッチするTensorflowをインストール  
という順でうまくゆきました。  
Tensorflowを後からインストールするところが大切かなと思います。  
という前提のもと、[https://www.tensorflow.org/install/source#common_installation_problems](https://www.tensorflow.org/install/source#common_installation_problems)　を見ます。  
ページは、LINUX用のソースからビルドするための情報ページですが、ここを参考にしました。  
GPUのところを見ます。  
当方環境の場合、CUDA 11.0.3 Update 1　まで可能なので、CUDAのバージョンが最も新しいものを選択すると  
一番上の  
* tensorflow-2.1.0
* Python バージョン　 2.7、3.5～3.7
* cuDNN 7.6
* CUDA 10.1
が適応できるのではないか、と判断します。  
ここで、NVIDIAから　CUDA TOOLKITのバージョン10.1とcuDNN 7.6 をダウンロードします。  
### Visual studio 2019のビルドツールのインストール
CUDA TOOLKITのインストールにはVisual studio 2019のビルドツールのインストールが必要らしいので  
Visual Studio のダウンロード サイトに移動します。  
再頒布可能パッケージおよびビルドツール を選択します。    
以下をダウンロードしてインストールします。  
* Microsoft Visual C++ 2019 再頒布可能ファイル
* Microsoft Build Tools 2019

### CUDA TOOL KIT をインストール
ダウンロード、インストールします。 
cuDNN 7.6 をダウンロード、配置、環境変数にpath設定 
ダウンロード、解凍、それぞれをCUDA TOLLKITのディレクトリにコピーします。  
.dllファイルのPATHを設定します。  
### Anaconda環境の新しいものを作成
Python バージョン　 3.7で新規環境を作成  
Anaconda環境にTensorflowのGPU版をインストール   
tensorflow-2.1.0をバージョン指定でインストールします。  
conda コマンドでインストールします。  
まず、  
```
conda search tensorflow
```
を入力します。すると、いろんなバージョンが出力されます
```
Loading channels: ...working... done
# Name                       Version           Build  Channel             
tensorflow                     1.7.0               0  pkgs/main           
tensorflow                     1.7.1               0  pkgs/main           
tensorflow                     1.8.0               0  pkgs/main           
tensorflow                     1.9.0 eigen_py35hb0e21f4_1  pkgs/main           
tensorflow                     1.9.0 eigen_py36h0b764b7_1  pkgs/main           
tensorflow                     1.9.0 gpu_py35h0075c17_1  pkgs/main           
tensorflow                     1.9.0 gpu_py36hfdee9c2_1  pkgs/main           
tensorflow                    1.10.0 eigen_py35h38c8211_0  pkgs/main           
tensorflow                    1.10.0 eigen_py36h849fbd8_0  pkgs/main           
tensorflow                    1.10.0 gpu_py35ha5d5ef7_0  pkgs/main           
tensorflow                    1.10.0 gpu_py36h3514669_0  pkgs/main           
tensorflow                    1.10.0 mkl_py35h4a0f5c2_0  pkgs/main           
tensorflow                    1.10.0 mkl_py36hb361250_0  pkgs/main           
tensorflow                    1.11.0 eigen_py36h346fd36_0  pkgs/main           
tensorflow                    1.11.0 gpu_py36h5dc63e2_0  pkgs/main           
tensorflow                    1.11.0 mkl_py36h41bbc20_0  pkgs/main           
tensorflow                    1.12.0 eigen_py36h67ac661_0  pkgs/main           
tensorflow                    1.12.0 gpu_py36ha5f9131_0  pkgs/main           
tensorflow                    1.12.0 mkl_py36h4f00353_0  pkgs/main           
tensorflow                    1.13.1 eigen_py36hf0a88a9_0  pkgs/main           
tensorflow                    1.13.1 eigen_py37h2a8d240_0  pkgs/main           
tensorflow                    1.13.1 gpu_py36h1635174_0  pkgs/main           
tensorflow                    1.13.1 gpu_py36h9006a92_0  pkgs/main           
tensorflow                    1.13.1 gpu_py37h83e5d6a_0  pkgs/main           
tensorflow                    1.13.1 gpu_py37hbc1a9d5_0  pkgs/main           
tensorflow                    1.13.1 mkl_py36hd212fbe_0  pkgs/main           
tensorflow                    1.13.1 mkl_py37h9463c59_0  pkgs/main           
tensorflow                    1.14.0 eigen_py36hf4fd08c_0  pkgs/main           
tensorflow                    1.14.0 eigen_py37hcf3f253_0  pkgs/main           
tensorflow                    1.14.0 gpu_py36h305fd99_0  pkgs/main           
tensorflow                    1.14.0 gpu_py36heb2afb7_0  pkgs/main           
tensorflow                    1.14.0 gpu_py37h2fabf85_0  pkgs/main           
tensorflow                    1.14.0 gpu_py37h5512b17_0  pkgs/main           
tensorflow                    1.14.0 mkl_py36hb88db5b_0  pkgs/main           
tensorflow                    1.14.0 mkl_py37h7908ca0_0  pkgs/main           
tensorflow                    1.15.0 eigen_py36h932cce6_0  pkgs/main           
tensorflow                    1.15.0 eigen_py37h9f89a44_0  pkgs/main           
tensorflow                    1.15.0 gpu_py36h2b26d6b_0  pkgs/main           
tensorflow                    1.15.0 gpu_py37hc3743a6_0  pkgs/main           
tensorflow                    1.15.0 mkl_py36h997801b_0  pkgs/main           
tensorflow                    1.15.0 mkl_py37h3789bd0_0  pkgs/main           
tensorflow                     2.0.0 eigen_py36h457aea3_0  pkgs/main           
tensorflow                     2.0.0 eigen_py37hbfc5123_0  pkgs/main           
tensorflow                     2.0.0 gpu_py36hfdd5754_0  pkgs/main           
tensorflow                     2.0.0 gpu_py37h57d29ca_0  pkgs/main           
tensorflow                     2.0.0 mkl_py36h781710d_0  pkgs/main           
tensorflow                     2.0.0 mkl_py37he1bbcac_0  pkgs/main           
tensorflow                     2.1.0 eigen_py36hdbbabfe_0  pkgs/main           
tensorflow                     2.1.0 eigen_py37hd727fc0_0  pkgs/main           
tensorflow                     2.1.0 gpu_py36h3346743_0  pkgs/main           
tensorflow                     2.1.0 gpu_py37h7db9008_0  pkgs/main           
tensorflow                     2.1.0 mkl_py36h31ad7c1_0  pkgs/main           
tensorflow                     2.1.0 mkl_py37ha977152_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37h04bc1aa_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37h10aaca4_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37h3bad0a6_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37h48e11e3_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37h856240d_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37h936c3e2_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37h952ae9f_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37he40ee82_0  pkgs/main           
tensorflow                     2.3.0 mkl_py37he70e3f7_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38h1fcfbd6_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38h37f7ee5_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38h3c6dea5_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38h46e32b0_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38h637f690_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38h8557ec7_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38h8c0d9a2_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38ha39cb68_0  pkgs/main           
tensorflow                     2.3.0 mkl_py38hd19cc29_0  pkgs/main           
```
2.1.0をインストールします。
tensorflow 2.1.0 gpu_py37h7db9008_0 pkgs/main　という行があります
&nbsp;
conda install tensorflow=2.1.0=gpu_py37h7db9008_0
&nbsp;
というコマンドを入力してインストールします。
Kerasなどをインストール
```
conda install keras
conda install matplotlib
conda install scikit-learn
conda install pandas
```
などをインストールします。
###GPUが認識されているか確認

Jupyter Notebook をインストールします。
新しいnoteを作ります
```
from tensorflow.python.client import device_lib 
device_lib.list_local_devices()
```
を入力、実行します
```
[name: "/device:CPU:0"
 device_type: "CPU"
 memory_limit: 268435456
 locality {
 }
 incarnation: 6994338167513753911,
 name: "/device:GPU:0"
 device_type: "GPU"
 memory_limit: 4990763008
 locality {
   bus_id: 1
   links {
   }
 }
 incarnation: 12190096428376131523
 physical_device_desc: "device: 0, name: GeForce GTX 1660 SUPER, pci bus id: 0000:01:00.0, compute capability: 7.5"]
```
のような情報が出力されます。この中にGPUというもブロックがあればGPUを認識しています。

## 感想
最初、Anaconda環境を作成後、`conda install Tensorflow`でインストールしたあとに、CUDA周りをインストールしたのですが、バージョンが合わずGPUを確認できませんでした。  
### GPUドライバ最新インストール
1. GPUバージョン確認
1. CUDA, cuDNN, tensorflow GPUの組み合わせを確認・メモ　[https://www.tensorflow.org/install/source](https://www.tensorflow.org/install/source) 
1. Visual studio 2019のビルドツールインストール 
1. CUDA toolkit インストール  
1. cuDNN　インストール  
1. Anaconda環境　Pythonのバージョン合わせて作成  
1. `conda search tensorflow` してからのバージョン指定インストール  
が近道かなと思います。  
cuDNNのインストールや、Visual studio 2019のビルドツールのインストール部分などの具体的な部分は検索するとたくさん情報があるので、割愛させていただきました。