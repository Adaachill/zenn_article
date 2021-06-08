---
title: "ABC204 C Tour[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","BFS","よくあるミス"]
published: True
---

# URL
https://atcoder.jp/contests/abc204/tasks/abc204_c

# 制約
$$ 2 <= N <= 2000$$
$$ 0 <= M<= min(2000,N(N-1))$$

# 問題概要
- 頂点数N、辺数Mの有向グラフが与えられる
- 0本以上の辺を通って移動することが可能なスタート地点とゴール地点の頂点の組の数を答えよ


# 提出コード
```python
# 有向グラフで、通行可能な頂点のペアの数を求める
# 2000だからN^3がギリだめ。N^2は余裕なので始点を固定してbfsをする
from collections import deque
n, m = map(int, input().split())
a = [0]*m
b = [0]*m
adj = [[] for _ in range(n)]
for i in range(m):
    c, d = map(int, input().split())
    a[i] = c-1
    b[i] = d-1
    adj[a[i]].append(b[i])
# bfs
ans = 0
q = deque()
for i in range(n):
    q.append(i)
    visit = [False]*n
    while q:
        start = q.popleft()
        ans += 1
        visit[start] = True
        for v in adj[start]:
            if visit[v]:
                pass
            else:
                q.append(v)
                visit[v] = True

print(ans)
```

# 考察
- 始点と終点どちらも固定できるかな？　-> O(N^3)で無理。片方固定すると？　O(N^2)でいける
- 始点を固定してBFSで全頂点に対して全探索することを全頂点に対して行う

# 実装方針
- BFSで書いた。
- ループがあるかもしれないが、visitですでに訪れた頂点を管理すれば問題はない。

# 次回への反省
- 最初に提出したコードがBFSの更新のバグでWA
  - visit を入れる場所をキューから取り出す時にしていた。
  - append の時にvisitを更新しないと現在みている頂点から移動可能な頂点達の中で同じ頂点へのパスを持つものがあるとそれが複数回追加されてバグる
```python:WA.py
# 有向グラフで、通行可能な頂点のペアの数を求める
# 2000だからN^3がギリだめ。N^2は余裕なので始点を固定してbfsをする
from collections import deque
n, m = map(int, input().split())
a = [0]*m
b = [0]*m
adj = [[] for _ in range(n)]
for i in range(m):
    c, d = map(int, input().split())
    a[i] = c-1
    b[i] = d-1
    adj[a[i]].append(b[i])
# bfs
ans = 0
q = deque()
for i in range(n):
    q.append(i)
    visit = [False]*n
    while q:
        start = q.popleft()
        ans += 1
        visit[start] = True # ここでvisitを更新すると1>2,1>3,2>4,3>4 のようなケースで4が2回queueに入る　
        for v in adj[start]:
            if visit[v]:
                pass
            else:
                q.append(v)
print(ans)

```
# 参考
