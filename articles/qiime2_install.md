---
title: "Qiime2のインストール"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["bioinfo"]
published: false
---

# Qiime2とは？
Qiime2はメタゲノム解析でよく用いられるOSS である。
シークエンスデータ（short read のamplicom sequence)から様々な解析、統計処理を行うことができる

# installとtutorial
日本語記事を読むとconda で入れるのが便利っぽい
Qiime2では、qzaに様々な入力データを保存する必要がある
tutorial ではfasta を取得した後、それをqzaに保存している
また、Qiime2では様々なインターフェースで実行することが可能で、
[pythonから実行することも可能]（https://qiita.com/K_microbiome/items/20aa10557b295042a719)

# Qiime2のコンセプト
[公式サイト](https://docs.qiime2.org/2020.8/concepts/)を参照すると、
1. Qiime2の特徴としては、データは全て分散型データ来歴を使うことで各データがどのようなコマンドを経て実行されたかを自動で記録されている。
具体的にはQiime2で生成されるデータは全てQiime2 artifacts という形式で存在し、Qiime2 artifactsはデータとファイルのタイプやどのようにそのデータが生成されたかの情報を含むメタデータを含む形で保存される。Qiime2 artifacts は.qza の拡張子で保存される。
2. データも同様に来歴を保持した上で美しく可視化できる。可視化データは.qzv で保存され、同様にメタデータとデータから構成される。
3. データのSemantic をメタデータとして保持することで、違う形式のデータを入力にすることがなくなる
<!-- 可視化したfigを簡単にダウンロードできるとかそんな感じか？　-->
4. Plugin-based system : Qiime2を通して様々な機能をプラグインとして使うことができる

# Qiime2のインターフェース
現在(2020/10/28閲覧)プロトタイプのq2studioを含め3つのインターフェースを用いてQiime2を利用できる
1. q2cli : コマンドライン
2. Artifact API : Python module として提供されpythonから利用できる
3. q2stdio: プロトタイプだがGUI

 # インストール
 [2020/10現在の公式ドキュメント](https://docs.qiime2.org/2020.8/)をみながらやっていく
Nativeに（そのまま）Qiime2をインストールするか、仮想環境（Dockerなど）に入れるかの2択

めんどくさくなったな。
