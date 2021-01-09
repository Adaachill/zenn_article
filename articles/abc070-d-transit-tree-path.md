---
title: "ABC070 D - Transit Tree Path解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","木","クエリ問題"]
published: true
---

# URL
https://atcoder.jp/contests/abc070/tasks/abc070_d

# 問題概要
- N頂点の木があり、それぞれの辺にはコストが与えられる
- Q個のクエリがあり、j番目のクエリではxj から頂点Kを経由しyj まで移動する場合の最短経路を出力する

# 提出コード
```python
from collections import deque

n = int(input())
a, b, c = [0] * (n - 1), [0] * (n - 1), [0] * (n - 1)
# 隣接リスト 頂点i の持つ辺をe[i] で表す
e = [[] for _ in range(n)]
for i in range(n - 1):
    x, y, z = map(int, input().split())
    x -= 1
    y -= 1
    e[x].append((y, z))
    e[y].append((x, z))

q, k = map(int, input().split())
k -= 1
# 頂点Kを根としたときの、任意の頂点からその頂点の親で根から距離1の頂点のindex
ancester = [-1] * (n)  # 頂点K は-1 にする
cost_to_k = [0] * (n)

Q = deque()
Q.append(k)
depth = 0
while Q:
    now = Q.popleft()
    depth += 1
    for child, edge_cost in e[now]:
        if ancester[child] == -1:  # unvisited
            if now == k:
                ancester[child] = child
            else:
                ancester[child] = now
            Q.append(child)
            cost_to_k[child] = cost_to_k[now] + edge_cost
# print(cost_to_k, ancester)
for i in range(q):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    print(cost_to_k[a] + cost_to_k[b])


```

# 考察
- クエリ問題は各クエリに共通して必要な値を前処理で計算することで計算量を落とす
- ~~最短経路と書いてあるが木でループは存在しないので、xi からyiまでのパスにKが存在すればそれが答えになり、存在しないならxi またはyiのうちKに近い方からKまでのパス*2を答えに足せば良い~~ Kの位置にかかわらずxi,yiからKまでのコストを足したものを出せば良い
- 前計算として全頂点からKまでの距離~~と、任意の二頂点の間にKが含まれるか~~の情報が欲しい
- ~~頂点Kを根としたときの、任意の頂点からその頂点の親で根から距離1の頂点のindexを事前計算してもてばO(1)で計算できる~~ 必要ない

# 実装方針
- 頂点Kからの距離は頂点KからBFSをすれば良いO(N)
- ~~BFS時に子ノードは親ノードのindex を代入するようにするとその頂点の親で根から距離1の頂点のindexがわかる depth_1[i]~~
- 各クエリに対しては、~~depth_1の値が一致すれば|cost[i]-cost[j]|を返し、一致しなければ~~cost[i]+cost[j]を返す

# メモ
- 任意の二頂点の間にKが含まれるかの情報をどうやって前計算するかが今回のポイント
- 明らかに誤読していた。。単純にk からのコストを計算してcost[i]+cost[j]を出力すればいいだけ



# 参考
