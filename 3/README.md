# 解析環境の構築

RNA-seq解析ツールの使い方

## パッケージ管理ツールやコンテナの利用

- パッケージ管理ツール: Anaconda (miniconda, miniforge), Homebrew (Linuxbrew)
- コンテナ(コンテナ型仮想化): Docker, Singularity (Apptainer)

パッケージ管理ツールとは、インストール、バージョン管理、依存関係 (ツール相互の利用) を解決するもの

コンテナとは、OSからツールから全部入りでまるごとそのツールを実行するための環境を提供するもの

## 今回利用するもの
- Apptainer
- Miniforge/Miniconda（簡単な紹介に留めます）

## RNA-seq解析環境

この後の解析実習では以下のツールを使う

- HISAT2
- StringTie
- Samtools

Apptainerコンテナを利用する方法を紹介（Miniforge/Minicondaの仮想環境にインストールする方法は簡単に紹介）


## Apptainerコンテナの利用

### コンテナの場所

`/usr/local/biotools/ツールの頭文字`以下にコンテナがある

HISAT2

```
ls /usr/local/biotools/h/hisat2*
```

StringTie

```
ls /usr/local/biotools/s/stringtie*
```

Samtools

```
ls /usr/local/biotools/s/samtools*
```

バージョンたくさんあるが、一番新しいものを使う(動かないものもあるので、そのときは別のバージョンを使う)

### コンテナを動かす

`singularity exec オプション コンテナのフルパス ツールのコマンド ツールのオプション`

というかたちで動かす

以下を1行で入力、実行

```
singularity exec /usr/local/biotools/s/stringtie:3.0.1--h00789bb_0 stringtie -h
```

実行すると以下のヘルプメッセージが出力される

```
StringTie v3.0.1 usage:

stringtie <in.bam ..> [-G <guide_gff>] [-l <prefix>] [-o <out.gtf>] [-p <cpus>]
 [-v] [-a <min_anchor_len>] [-m <min_len>] [-j <min_anchor_cov>] [-f <min_iso>]
 [-c <min_bundle_cov>] [-g <bdist>] [-u] [-L] [-e] [--viral] [-E <err_margin>]
 [--ptf <f_tab>] [-x <seqid,..>] [-A <gene_abund.out>] [-h] {-B|-b <dir_path>}
 [--mix] [--conservative] [--rf] [--fr]
Assemble RNA-Seq alignments into potential transcripts.

(以下略)
```

自分のhomeディレクトリ以外のファイルを読み込みたい時は、`-B` オプションでbindする


















