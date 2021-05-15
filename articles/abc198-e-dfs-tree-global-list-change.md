---
title: "ABC198 E Unique Color [python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","木のDFS","DFS"]
published: True
---

# URL
https://atcoder.jp/contests/abc198/tasks/abc198_e

# 制約
$$2 <= N <= 10^{5}$$
$$ 1<= C[i]<= 10^{5} $$
$$1 <= A[i],B[i] <= N$$
グラフは木

# 問題概要
- N頂点からなる木がある
- 各頂点は色C[i]に塗られている
- 頂点1から頂点xへの最短パスにx と同じ色がx以外ないような時xは良い頂点と定義する
- 良い頂点を全て求めよ

# 提出コード
```python
from typing import List
import sys
sys.setrecursionlimit(2*10**5)

n = int(input())
C = list(map(int, input().split()))
adj: List[List[int]] = [[] for _ in range(n)]
for i in range(n-1):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    adj[a].append(b)
    adj[b].append(a)

visit = [False]*(n+1)
color = [False]*(10**5+100)  # 頂点1からの最短パスにある色があるならTrue
ans: List[int] = []


def dfs(v: int) -> None:

    origin_state: bool = visit[v]
    origin_color: bool = color[C[v]]
    if color[C[v]] == False:
        ans.append(v+1)  # 1-index に戻して入れる
    visit[v] = True
    color[C[v]] = True
    # 子の頂点に行く時はvisit の状態はすでに訪れたようにしておく
    for next_v in adj[v]:
        # 親ノードがきたら飛ばす
        if visit[next_v]:
            continue
        dfs(next_v)
    visit[v] = origin_state  # 次の頂点に行く時は元に戻す
    color[C[v]] = origin_color


dfs(0)
ans.sort()
for i in ans:
    print(i)

```

# 考察
- 全部の頂点から頂点1までの最短パスを求めるとO(N^2)になって TLEする
  - 親ノードに情報を持ちたい
  - 1からの最短パス上の出現した色のリストをもつとO(N^2)になって結局だめ
  - １つの配列を探索時に使いまわしたい
  - 木のDFSで探索時に書き換えるようにする
    - 次の頂点を行く時に、今の頂点の状態を一時的にもち、今の頂点をvisitに加える
    - 帰ってくる時に一時的に持っていたもとの状態に戻す

# 実装方針
1. 木の入力(隣接リスト)
2. 子に移動するための配列visitと,path上の色を管理する配列colorをグローバルに用意する
3. dfsをして、visitとcolorの元の状態を子ノードに行く前に一時的に記録して子ノードから戻ってきたら元に戻すようにするようにして2つの配列を変更していく
4. sortして昇順にして出力


# 次回への反省
- TLEしそうなケースとして一直線でTLEしないやり方をまず考える
- 木でリストのコピーは使わない
  - 1つの配列を使い回す
    - DFS でグローバルにおいた配列を更新していく。
- path0~ のテストケースでREを出した。
  - REが出た時の常道として配列外参照を疑ったが、再帰回数の上限を変更するのを忘れていた。
  - sys.setrecursionlimit(2*10**5) で解決
  - 再帰を書くときは再帰上限も変更する!
  

# 参考
- 