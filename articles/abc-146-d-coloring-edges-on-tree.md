---
title: "ABC 146 D - Coloring Edges on Tree解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","木","構築"]
published: true
---

# URL
https://atcoder.jp/contests/abc146/tasks/abc146_d

# 問題概要
- N頂点の木があり、辺は全て与えられる
- 木の辺を各頂点について、その頂点を端点に持つ辺の色がすべて相異なるように塗り分ける時、使う色の数が最小になるようなものを１つ構築して出力しろ。

# 提出コード
```python
from collections import deque

n = int(input())
x = [[] for _ in range(n)]  # 隣接リスト
a, b = [[] for _ in range(n)], [[] for _ in range(n)]
for i in range(n - 1):
    a[i], b[i] = map(int, input().split())
    a[i], b[i] = a[i] - 1, b[i] - 1
    x[a[i]].append((b[i], i))
    x[b[i]].append((a[i], i))  # 辺のidex を0index でタプルで保持
# 0 からdfs
edge = [0] * (n - 1)  # 各辺の色を1 indexで持つ
parent = [-1] * (n)  # それぞれの頂点が親からの辺の色を持つ
q = deque()
q.append(0)
num_color = 0
# 頂点でBFS
while q:
    now = q.popleft()
    color = 1
    ng_color = parent[now]
    for v, e in x[now]:
        if edge[e] == 0:
            if color == ng_color:
                color += 1  # 親からの辺と同じ色になったら変える
            num_color = max(num_color, color)
            edge[e] = color
            parent[v] = color
            color += 1
            q.append(v)
print(num_color)
for i in range(n - 1):
    print(edge[i])

```

# 考察
- 直感的に次数最大の頂点の次数が答えになりそう
 - 証明：木でループ（閉路）は存在しないので、各頂点ごとに考えれば良い。（隣の頂点を端点とする辺がどんな色を使っていようが、それが現在の頂点を端点としないなら自由に色を使える）

# 実装メモ
- あとは簡単な構築の方法を考える
  - 頂点ごとにその頂点が持つ辺を全探索するという動作を繰り返したいので、BFSをする
  - 各辺に対して現在の色（または決まっていない）をリストでもつ
  - 入力で隣接リストで各頂点と隣接する頂点を持つとともに、その辺のindex　をタプルで取得できるようにする
  - 親からの辺がどの色であるかを変数として持つ（その色はskipする必要があるため）

# メモ
- 親からの辺の色を持つ必要があることを実装中に気づいた。
- 頂点でDFSをしているのに、辺でDFSをしている感覚（?)になり少し戸惑った


# 参考
