---
title: "GCJ Qualification Round 2021参加記[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","連結成分分解","BFS"]
published: false
---

# URL
https://codingcompetitions.withgoogle.com/codejam/round/000000000043580a

# Reversort (7pts)
## 問題概要
- 

## 制約
$$1 <= N,M,Q <=50$$
$$1 <= W[i],V[i],X[i] <=10^{6}$$

# 提出コード
```python

```

# 考察
- 制約から計算量を考えると、N,M,Qが小さいのでO(NM^2Q)くらいまでは通りそうだとわかる
- ナイーブな方法を考えて、それぞれの箱にどの荷物が入るかを考えてnCm*Qで計算できる
-  

# 実装
- 全頂点に対してループをして現在見ていない頂点に対してBFSをすることで連結成分に分解することができる
- 連結成分に関して、辺の数eと頂点数vをカウントすると、ans += min(v,e) で答えがもとまる
- 自己辺が存在するので、自己辺以外は二回カウント、自己辺は一回カウントしてそれを二で割ると正しい辺の数になる