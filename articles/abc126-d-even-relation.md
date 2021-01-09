---
title: "ABC126 D - Even Relation解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","木","構築"]
published: true
---

# URL
https://atcoder.jp/contests/abc126/tasks/abc126_d

# 問題概要
- N頂点の木が辺とともに与えられ、各辺にはコストがある。
- 条件を満たすように木を２色に塗り分ける時その塗り分け方を１つ出力せよ
- 条件：同じ色に塗られた任意の2頂点間のコストが偶数

# 制約
N<=10**5

# 提出コード
```python
#!/usr/bin/env python3
from collections import deque

n = int(input())
x = [[] for _ in range(n)]
color = [-1] * n
for i in range(n - 1):
    u, v, w = map(int, input().split())
    u -= 1
    v -= 1
    x[u].append((v, w))
    x[v].append((u, w))
q = deque()
q.append(0)
color[0] = 0
while q:
    now = q.popleft()
    # print(color)
    for v, c in x[now]:
        if color[v] != -1:
            continue
        if c % 2:
            color[v] = 1 - color[now]  # bit反転 = (-x+1)
        else:
            color[v] = color[now]
        q.append(v)

for i in range(n):
    print(color[i])

```

# 考察
- 全てのコストが奇数ならBFSをして隣接する頂点を違う色にすれば良い
- 普通にBFSするだけで解けるのでは？（ループ内での整合性をとる必要がないので）
  - 証明：隣接する頂点に対してそれをつなぐ辺のコストが奇数なら違う色、偶数なら同じ色にする操作を考える。木グラフのため頂点は一度ずつしか訪れない。隣接する頂点に対してはこの操作で条件を満たす。隣接しない頂点に対しては制約がないため満たす。

# 実装方針
- 各頂点の隣接グラフを持つ
- 各頂点の色をリストでもつ(visitedかもここで判定)
- 頂点0からBFS(任意の頂点が出発点でもOK)


# 次回への反省
- pythonのbit演算について調べたので頭に入れる
- ```&:=AND,|:=OR, ^:=XOR,~:=NOT,<<,>>:= bit shift``` 
- queue に追加するのを忘れるという凡ミスでバグ産んでた。
# 参考
- https://note.nkmk.me/python-bit-operation/ Python のbit演算について