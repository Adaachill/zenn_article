---
title: "ABC188 E - Peddler解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","BFS"]
published: true
---

# URL
https://atcoder.jp/contests/abc188/tasks/abc188_e

# 問題概要
- 1..Nの町があり、大きい価の町に向けてのみM 本の道が存在する
- それぞれの町にはai のコストを持つ。
- aj-ai の最大値を求めよ（ただしi,jはai からaj までのpathが存在する）

# 制約
$$ 1<=N <= 2*10^{5}$$
$$ 1 <= M <= 2*10^{5}$$

# 提出コード
```python
import sys

sys.setrecursionlimit(1000000)
INF = 10 ** 9
n, m = map(int, input().split())
a = list(map(int, input().split()))
e = [[] for _ in range(n)]
e_rev = [[] for _ in range(n)]
for i in range(m):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    e[x].append(y)
    e_rev[y].append(x)
# 順方向にdfs　をして現在の頂点の子のmax を計算
# 逆方向にdfsをして現在の頂点の親のmin を計算
max_child = [-INF] * n
min_parent = [INF] * n


def dfs(v):
    if max_child[v] != -INF:
        return max_child[v]
    val = -INF  # 子がいない時は0にする
    for i in e[v]:
        val = max(dfs(i), val, a[i])
    max_child[v] = val
    # print(v, val)
    return val


def dfs_rev(v):
    if min_parent[v] != INF:
        return min_parent[v]
    val = a[v]  # こっちは自分自身の値で初期化
    for i in e_rev[v]:
        val = min(dfs_rev(i), val, a[i])
    min_parent[v] = val
    return val


# print(max_child, min_parent)
ans = -INF
for i in range(n):
    max_child[i] = dfs(i)
    min_parent[i] = dfs_rev(i)
    ans = max(ans, max_child[i] - a[i])
print(ans)

```

# 考察
- それぞれの頂点に対して、現在の頂点の子のmax、現在の頂点の親のminを計算すれば良い
  - 順方向にdfs　をして現在の頂点の子のmax を計算
  - 逆方向にdfsをして現在の頂点の親のmin を計算
- よく考えると最小値になるときは自分がmin になっている頂点を起点になるので、逆方向のdfsはいらなくてmax_child[i] - a[i]　のみで良い