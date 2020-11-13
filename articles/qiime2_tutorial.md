---
title: "ペアエンドメタゲノムシークエンスデータからQiime2で一通りの解析を行う"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["bioinfo"]
published: false
---

# Qiime2とは？
メタゲノム解析でよく用いられるツールです。詳しくは[前回の記事](https://zenn.dev/knk_kei/articles/qiime2_install)を参照して下さい

# Qiime2基本
データの可視化


# Qiime2でできることの概要
![](https://docs.qiime2.org/2020.8/_images/overview.png)
*Qiime2公式ドキュメントより*
## ワークフロー概要
1. fasta,fastqなどでmetagenome/amplicom sequence dataを読み込み
2. それぞれのデータとサンプルを対応させるためにdemuliplexをする
3. denoised 処理（シークエンシングエラーや重複したシークエンスを覗くために）ASVやOTUのクラスタにまとめる(feature table or representative sequence)
4. feature table から様々な処理を行う(系統分類、alpha,beta多様性など)

# Qiime2で実際に解析する
## dataのimport[demux-paired-end.qzaの生成]
1. [このforumの記事](https://forum.qiime2.org/t/importing-and-demultiplexing-sequence-data-quick-reference/14002)が参考になる
2. 今回のケースでは既にdemultiplexされているデータを扱う（1サンプルにつきpair-endで2ファイルずつ存在する）
3. Illumna でシークエンスされたものであるのでCasava1.8 paire-end demultiplexed fastq として扱う。[公式ドキュメント](https://docs.qiime2.org/2020.2/tutorials/importing/#casava-1-8-single-end-demultiplexed-fastq)に従う
4. Casava format で生成されたものだと　fastqのファイルが アンダーバーで区切られ、最初からそれぞれ
1:sample の名前
2:barcode sequence or barcode identifier(数字のみor　S40とか)
3:lane number 
4:read の方向(R1 or R2)
5:the set number
になっている

Q. これ読み込むときに16s,ITS,mWGSを混ぜてもいいの？とりあえずやってみる

## ノイズ除去とクラスタリング
### 概要
QV に基づき？エラーっぽいreadを弾く
複製されたread を除く（ただしcountはするので安心して）
OTU skipping(現在は使われない？)
### qiime2で使われるtool(dada2とDeblur)
ノイズの除去やペアエンドの結合、duplicateの消去などを行う
これらの処理を行ったものを[ASV](https://en.wikipedia.org/wiki/Amplicon_sequence_variant)と呼ぶ


2. metadataをQiime2用に作成する。作り方については[以下](https://docs.qiime2.org/2020.8/tutorials/metadata/)を参照
3. 



# "Moving Pictures" tutorial 
2011 Caporaso et.alの研究のデータを使って一通りの解析を行う
データは二人の患者から5つのタイムポイントで体の4箇所から採取したDNAを16s rRNA でアンプリコンシークエンスしたものを使う（プライマーは16s rRNA geneのV4領域、シークエンサーはIllumina Hiseq)

# Atacama Tutorial
Pair end の tutorial
1. qiime dumux でdemultipleする。（casava 形式で既にdemultiplexed されたものを持っている場合はskip)
2. qiime demux summarize で統計量をみる 
```
qiime demux summarize --i-data demux-paired-end.qza --o-visualization demux.qzv
```
3. フィルター（opptional)
統計量からサンプルごとのread数やF,Rのread数が見れる
それを参考にフィルターをかけるならそれをやる
ここでmetadata-fileが必要になるんか。exportで自動で生成されるわけではないよね。。
```sample.sh
qiime demux filter-samples \
  --i-demux demux-subsample.qza \
  --m-metadata-file ./demux-subsample/per-sample-fastq-counts.tsv \
  --p-where 'CAST([forward sequence count] AS INT) > 100' \
  --o-filtered-demux demux.qza
```

seqkit で両端のbp を調べる
sample_metadataで統一する