# バッチジョブ
#### 国立遺伝学研究所　森宙史

### バッチジョブとは?

- コマンド入力と応答というようなインタラクティブな逐次処理ではなく、一連の処理を最初に入力した後、順次処理を実行するような処理の形式  
- 上記は、UNIXコマンドを一つ一つ入力して処理を実行する形式と、シェルスクリプトを記述して一気に処理を実行する形式、として考えることができる    
- つまり、バッチジョブの一例が、シェルスクリプトを記述して一気に処理を実行することである  　 
　  
### 遺伝研スパコンにおけるバッチジョブの実行方法
シェルスクリプトで処理を記述した上で、ジョブスケジューラであるSlurmを用いてsbatchコマンドでシェルスクリプトを実行させる。

詳しくは、  
- [Slurmの概要](https://sc.ddbj.nig.ac.jp/guides/software/JobScheduler/Slurm/)
- [バッチジョブの使い方](https://sc.ddbj.nig.ac.jp/guides/software/JobScheduler/Slurm/batch_jobs/)
- [その他のコマンド](https://sc.ddbj.nig.ac.jp/guides/software/JobScheduler/Slurm/other_commands/)
の3つを読めば大体わかる。  
　  
### 遺伝研スパコンにおけるバッチジョブの実行

1. FileZillaで遺伝研スパコンのホームディレクトリ上に、下記のbatch1.shファイルを作成

batch1.sh
```
#!/bin/bash
#SBATCH -t 0-00:10:00
#SBATCH --mem-per-cpu 4g
#SBATCH -J batch1
sleep 7
hostname > hos.txt

```

2. TeraTerm or ターミナル等を用いてsshで遺伝研スパコンにログインし、

`ssh ユーザーアカウント名@gw.ddbj.nig.ac.jp`

`ssh a001`

下記のコマンドを実行  

`chmod u+x batch1.sh`  
`./batch1.sh`  
`cat hos.txt`  
`sbatch batch1.sh`  
`squeue`  
`cat hos.txt`  

4. コードの解説  
`#SBATCH` はsbatchコマンドに渡すオプションを記述  
`-t` はジョブの実行時間の上限（この場合は10分）を指定  
`--mem-per-cpu` は使用するメモリ量（この場合は4GB）を指定  
`-J` はジョブの名前を指定  
`>` は左側のコマンドの結果を右側のファイル名のファイルに書き込む操作


### ジョブの実行状態および遺伝研スパコンの混雑状況の確認方法
`squeue` 

がジョブの実行状態を確認するコマンド

r	ジョブ実行中

qw	ジョブは実行待機中

t	ジョブは実行ホストへ転送処理中

順番待ちの状況だとqwになる。


どのくらい遺伝研スパコンが混んでいるかは、

`qstat -g c` 

で見ることができる。例えば、以下
```
CLUSTER QUEUE                   CQLOAD   USED    RES  AVAIL  TOTAL aoACDS  cdsuE  
--------------------------------------------------------------------------------
epyc.q                            0.20    952    128   1925   3200    128    256 
gpu.q                             0.12     63      0    369    432      0      0 
intel.q                           0.08    160    224    608    928      0    160 
login.q                           0.10     34      0    350    384      0      0 
login_gpu.q                       0.04      0      0     48     96      0     48 
medium-ubuntu.q                   0.24    251     80    309    560     80      0 
medium.q                          0.33    116     80    124    240      0      0 
short.q                           0.00      0      0    128    128      0      0
```

USED :現在使用中のスロット数

RES :事前予約(Advance Reservation)のスロット数

AVAIL :使用可能なスロット数

TOTAL :スロット総数

大まかには、AVAIL/TOTALの値が小さいほど混雑している。


### ジョブの削除方法
既に実行中または順番待ち中のジョブを終了を待たずに削除したい場合は qdel コマンドを使用する。 
