---
title: Windows11のRTX3060でAnaconda環境にてTensorflowのGPU版を使う方法
---
# Windows11のRTX3060でAnaconda環境にてTensorflowのGPU版を使う方法
Windows11でRTC3060をGPUに使っている環境で、Anaconda環境にてTensorflowのGPU版を使用可能にする環境構築の方法です。
Anaconda環境はじめメジャーなインストール方法やPATHの設定など細かいところは、検索で解決すると思い割愛させていただきました。  
うまくGPU認識させるためのインストール順とその根拠の確認が重要です、その点について書いています。
## OSとハードウェア
* OS: windows11  
* Computer: Z4 G4(HP)
* GPU: RTX3060(NVIDIA)
## GPUを認識できたTensorFlow 関連のソフトウェアバージョン
上記のOSとハードウェアで動いた（GPU認識できた）ソフトウェアの組み合わせです。  
Anaconda環境でのみの確認です。  
### Anaconda 環境
* Python3.9.15
* CUDA TOOLKIT 11.8
* Visual studio 2022
* cuDNN 
* tensorflow-2.9.1 (バージョン指定でGPU有でpython3.9.15に合わせる）
### インストール順・考え方など
GPUを利用するには、GPUボード（通常はグラフィックカード）が物理的に必須です。   
ソフトウェアはインストールやり直し作業だけですみますが、グラボをじゃんじゃん買い換えるわけにはいかないですよね。   
なので、スタートはGPUのドライバから始めることにします。
## Anaconda環境をインストール 新環境はのちほど
最初にいわゆるAnaconda環境をインストールしておきます。  
baseのみで大丈夫です、新たな環境などは後程インストールします。  
## NVIDIAのGPUドライバの最新版をインストール
今回はRTX3060の最新版をダウンロードインストールします。  
ダウンロード画面に  
バージョン: 527.56 WHQL　などの表記があるので、527.56　部分をメモしておきます。  
ダウンロードインストールします。  
![](/images/driver1.png)  
## CUDA TOOLKITの適切なバージョンを確認
NVIDIAのCUDA TOOLKITの適切なバージョンを確認します  
このページを見ます[https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)  
CUDA Driver　の表を見ます。GPU　RTX3060ドライバのバージョンだと527.56ですので  
CUDA 11.0 (11.0.3)　より上のバージョンが該当することが確認できます。　　
CUDA 11.0 (11.0.3)以上のバージョンであれば大丈夫なんだなと認識します。  

![](/images/cudatoolkit1.png)   

ダウンロードはまだしません。
ここで、自分のハードウェア環境とTensorflowとの各バージョンの組み合わせを判定します。  
TensorflowでGPUを認識させるには、  
* GPUのバージョン
* CUDAのバージョン
* cuDNNのバージョン
* Pythonのバージョン
が事前に正しい組み合わせであることが必要です。  
1.これら4種を正しい組み合わせでインストール  
2.その組み合わせにマッチするTensorflowをインストール  
という順でうまくゆきました。  
Tensorflowを後からインストールするところが大切かなと思います。  
という前提のもと、[https://www.tensorflow.org/install/source#common_installation_problems](https://www.tensorflow.org/install/source#common_installation_problems)　を見ます。  
LINUX用のソースからビルドするための情報ページですが参考になります。  
GPUのところを見ます。  
![](/images/gpu1.png)
## 適切なバージョン組み合わせを判定
当方環境の場合、CUDA 11.0 (11.0.3)　まで可能なので、CUDAのバージョンが最も新しいものを選択すると  
一番上の  
* テンソルフロー-2.11.0
* Python バージョン　 3.7-3.10
* cuDNN 8.1
* CUDA 11.2
が適応できるのではないか、と判断します。  
ここで、NVIDIAから　CUDA TOOLKITのバージョン11.2とcuDNN 8.1 をダウンロードします。  
### Visual studio 2019のビルドツールのインストール
CUDA TOOLKITのインストールにはVisual studio のビルドツールのインストールが必要らしいので  
Visual Studio のダウンロード サイトに移動します。  
再頒布可能パッケージおよびビルドツール を選択します。    
以下をダウンロードしてインストールします。  
* Microsoft Visual C++ 2022 再頒布可能ファイル
* Microsoft Build Tools 2022　 [https://visualstudio.microsoft.com/ja/visual-cpp-build-tools/](https://visualstudio.microsoft.com/ja/visual-cpp-build-tools/)

### CUDA TOOL KIT をインストール
ダウンロード、インストールします。 
cuDNN 7.6 をダウンロード、配置、環境変数にpath設定 
ダウンロード、解凍、それぞれをCUDA TOLLKITのディレクトリにコピーします。  
.dllファイルのPATHを設定します。  
### Anaconda環境の新しいものを作成
ここでAnaconda環境を用意します  
Python バージョン　 3.9.15で新規環境を作成  
Anaconda環境にTensorflowのGPU版をインストール   
tensorflow-2.11.0をバージョン指定でインストールします。  
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
tensorflow                     1.8.0      haa95532_0  pkgs/main
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
tensorflow                     2.5.0 eigen_py37hba85c30_0  pkgs/main
tensorflow                     2.5.0 eigen_py38h6b3c56f_0  pkgs/main
tensorflow                     2.5.0 eigen_py39hf7bd2bc_0  pkgs/main
tensorflow                     2.5.0 gpu_py37h23de114_0  pkgs/main
tensorflow                     2.5.0 gpu_py38h8e8c102_0  pkgs/main
tensorflow                     2.5.0 gpu_py39h7dc34a2_0  pkgs/main
tensorflow                     2.5.0 mkl_py37h99b934d_0  pkgs/main
tensorflow                     2.5.0 mkl_py38hbe2df88_0  pkgs/main
tensorflow                     2.5.0 mkl_py39h1fa1df6_0  pkgs/main
tensorflow                     2.6.0 eigen_py37h37bbdb1_0  pkgs/main
tensorflow                     2.6.0 eigen_py38h63d3545_0  pkgs/main
tensorflow                     2.6.0 eigen_py39h855417c_0  pkgs/main
tensorflow                     2.6.0 gpu_py37h3e8f0e3_0  pkgs/main
tensorflow                     2.6.0 gpu_py38hc0e8100_0  pkgs/main
tensorflow                     2.6.0 gpu_py39he88c5ba_0  pkgs/main
tensorflow                     2.6.0 mkl_py37h9623b36_0  pkgs/main
tensorflow                     2.6.0 mkl_py38hdc16138_0  pkgs/main
tensorflow                     2.6.0 mkl_py39h31650da_0  pkgs/main
tensorflow                     2.8.2 eigen_py310h3184f71_0  pkgs/main
tensorflow                     2.8.2 eigen_py37h326eb71_0  pkgs/main
tensorflow                     2.8.2 eigen_py38h0b14ea6_0  pkgs/main
tensorflow                     2.8.2 eigen_py39h9b0e0cb_0  pkgs/main
tensorflow                     2.8.2 gpu_py310h5cc41f4_0  pkgs/main
tensorflow                     2.8.2 gpu_py37h39c650d_0  pkgs/main
tensorflow                     2.8.2 gpu_py38he639981_0  pkgs/main
tensorflow                     2.8.2 gpu_py39h5ca5225_0  pkgs/main
tensorflow                     2.8.2 mkl_py310h517747f_0  pkgs/main
tensorflow                     2.8.2 mkl_py37h31f2aba_0  pkgs/main
tensorflow                     2.8.2 mkl_py38h6f30489_0  pkgs/main
tensorflow                     2.8.2 mkl_py39hfd350ca_0  pkgs/main
tensorflow                     2.9.1 eigen_py310h7e6dd96_1  pkgs/main
tensorflow                     2.9.1 eigen_py310hd297198_0  pkgs/main
tensorflow                     2.9.1 eigen_py37h2d80419_1  pkgs/main
tensorflow                     2.9.1 eigen_py37hd324010_0  pkgs/main
tensorflow                     2.9.1 eigen_py38h1fa63f8_0  pkgs/main
tensorflow                     2.9.1 eigen_py38h9efb3fa_1  pkgs/main
tensorflow                     2.9.1 eigen_py39h296740f_1  pkgs/main
tensorflow                     2.9.1 eigen_py39ha6ebefe_0  pkgs/main
tensorflow                     2.9.1 gpu_py310h5ade2b3_0  pkgs/main
tensorflow                     2.9.1 gpu_py310hdfba57f_1  pkgs/main
tensorflow                     2.9.1 gpu_py37h12a4f0f_0  pkgs/main
tensorflow                     2.9.1 gpu_py37h1b18eab_1  pkgs/main
tensorflow                     2.9.1 gpu_py38h46f8375_1  pkgs/main
tensorflow                     2.9.1 gpu_py38h4a6fda8_0  pkgs/main
tensorflow                     2.9.1 gpu_py39h05f4d00_1  pkgs/main
tensorflow                     2.9.1 gpu_py39hb21c0df_0  pkgs/main
tensorflow                     2.9.1 mkl_py310h0b323c9_0  pkgs/main
tensorflow                     2.9.1 mkl_py310h626feff_1  pkgs/main
tensorflow                     2.9.1 mkl_py37h6343fec_1  pkgs/main
tensorflow                     2.9.1 mkl_py37ha747b87_0  pkgs/main
tensorflow                     2.9.1 mkl_py38h7f03810_0  pkgs/main
tensorflow                     2.9.1 mkl_py38hff71f30_1  pkgs/main
tensorflow                     2.9.1 mkl_py39hb9887a6_0  pkgs/main
tensorflow                     2.9.1 mkl_py39hc9ebea8_1  pkgs/main
    
```
2.9.1をインストールします。
tensorflow                     2.9.1 gpu_py39hb21c0df_0  pkgs/main　という行があります  
※gpu_xxx とgpuで始まっているのがGPU版です。  
&nbsp;
conda install tensorflow=2.9.1=gpu_py39hb21c0df_0
&nbsp;
というコマンドを入力してインストールします。  

## GPUが認識されているかをJupyter Notebookで確認

Jupyter Notebookで新しいnoteを作ります
下記のコマンドを実行します。
```
from tensorflow.python.client import device_lib 
device_lib.list_local_devices()
```
を入力、実行します。  
すると
```
[name: "/device:CPU:0"
 device_type: "CPU"
 memory_limit: 268435456
 locality {
 }
 incarnation: 17267306238140247226
 xla_global_id: -1,
 name: "/device:GPU:0"
 device_type: "GPU"
 memory_limit: 10069475328
 locality {
   bus_id: 1
   links {
   }
 }
 incarnation: 12118592969954587719
 physical_device_desc: "device: 0, name: NVIDIA GeForce RTX 3060, pci bus id: 0000:21:00.0, compute capability: 8.6"
 xla_global_id: 416903419]
```
のような情報が出力されます。この中に device_type: "GPU" というブロックがあればGPUを認識しています。
![](/images/note-rtx3060.png)      

## まとめ
 Tensorflow　は最後にインストールすること  
conda search tensorflow　コマンドで一覧を出力し、gpu_で始まるGPU対応版をインストールすること  
が重要と思います。
失敗例では、Anaconda環境を作成後、`conda install Tensorflow`で最初にTensorflowをインストールしたら、他のバージョンと合わず失敗しました。

### 手順まとめ
1. GPUバージョン確認
2. CUDA, cuDNN, tensorflow GPUの組み合わせを確認・メモ　[https://www.tensorflow.org/install/source](https://www.tensorflow.org/install/source) 
3. Visual studio のビルドツールインストール 
4. CUDA toolkit インストール  
5. cuDNN　インストール  
6. Anaconda環境　Pythonのバージョン合わせて作成  
7. `conda search tensorflow` してからのバージョン指定インストール  
が近道かなと思います。  
cuDNNのインストールや、Visual studio のビルドツールのインストール部分などの具体的な部分は検索するとたくさん情報があるので、割愛させていただきました。  
## おまけ
Kerasとかも　
```
conda install keras
conda install matplotlib
conda install scikit-learn
conda install pandas
```
などをインストールしました。

[HOME](index.md)
 