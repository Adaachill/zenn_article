---
title: "ABC199 D RGB Coloring解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python",]
published: false
---

# URL
https://atcoder.jp/contests/abc199/tasks/abc199_d

# 制約
$$　1 <= N <= 20　$$
$$ 0　<= M<= N*(N-1)/2 $$
$$　1 <= A[i] <= N　$$
$$　1 <= B[i] <= N　$$
グラフは多重辺や自己ループなし

# 問題概要
- N頂点M辺の単純無向グラフがある
- グラフの各頂点を３色で塗る方法で以下の条件をみたす塗り方の個数を答えよ
- 条件：直接辺で結ばれている二頂点は違う色

# 提出コード
```python

```

# 考察
解説AC
- Nが小さいので計算量に関しては結構無理できそう
  - 3**20 は10^9 オーダーでギリギリだめ。
解説
- ３色の一色の場所を固定する。
  - その色を使うか、使わないかの2択で2^N 
  - 使う頂点が直接結ばれていれば条件を満たさずだめ
  - 使わない頂点をどう塗り分けるかに関しては、端点の色さえ決めればあとは一意に決まる


# 実装方針
1. 一つの色に関して、2^Nの全探索をbit全探索で行う
2. 使う頂点を始点にしてDFSをする
- 固定した色に関しての隣接判定は、親の色と比較することで判定する
- 固定した色以外に関しては、端点の色だけ固定して塗り分けれるか判定 ans*2する
- DFSに渡す引数としては、親の頂点、現在までの塗り分け方


# 次回への反省
- 

# 参考