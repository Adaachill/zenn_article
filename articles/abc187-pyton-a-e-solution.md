---
title: "ABC187 メモ[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python"]
published: true
---

# A - Large Digits 
各位の和が大きいものを出力
- str に対してはそのままリスト内包表記が使える
# B - Gentle Pairs 
xy 平面上のN点があり、その中から傾きが-1以上1以下になる組みを求めよ
- O(N^2)で全探索
# C - 1-SAT 
##　概要
Nこの文字列集合Sがあり、それは英子文字からなる文字列に!を1以下個追加したもの
文字列Tが!を追加、あるいはそのままでSに含まれるならその一致する文字列を出力せよ
## 考察
- Sに含まれる文字列は!は先頭に一個以下しかないのでTの先頭は英子文字である。
- つまりS中に!を除いて同じ文字列があるかどうかをO(N)or O(NlogN)で調べる問題
- あらかじめ、!がついていないS中の文字列をソートして比較すればいい

初見でわからなくてかなりパニクった。

## 提出コード
```python
import bisect

n = int(input())
s = [input() for _ in range(n)]
ref = [i for i in s if i[0] != "!"]
query = [i for i in s if i[0] == "!"]
ref.sort()
# print(ref, query)
for s in query:
    top = bisect.bisect_left(ref, s[1:])
    end = bisect.bisect_right(ref, s[1:])
    if top != end:
        print(s[1:])
        exit()
print("satisfiable")
```

# D - Choose Me
## 考察
- 町i で演説をした時の[高橋票-青木票]の増加量は a[i]+2*b[i]
- 最小の数を求めるので　a[i]+2*b[i]　でソートして降順に選んでいくと最適

## 提出コード
```python
n = int(input())
a, b, c = [0] * n, [0] * n, [0] * n
for i in range(n):
    a[i], b[i] = map(int, input().split())
    c[i] = 2 * a[i] + b[i]
c, a, b = zip(*sorted(zip(c, a, b)))
ans = 0
hyo = -sum(a)
# print(hyo, c, a, b)
while hyo <= 0:
    hyo += c[n - 1 - ans]
    ans += 1
print(ans)
```
##　コメント
inputを逆にしていてデバッグで時間を食った

# E - Through Path 
## 考察
- 各クエリに対して更新される可能性がある頂点数はO(N)なので、木DPを使う
  - 部分木のrootになる頂点に対して値を保存して、その部分木には全て同じ操作をするようにする
  - 最後にまとめてrootから計算する
- 式としては
  - 起点になる頂点(1でのa,2でのb)がrootに近いならrootに+xi,b[i]に-x[i],a[i]の頂点に-x[i]
  - rootから遠い方なら a[i]に+x[i],a[i]の頂点に-x[i]

## 実装
- 木DPに関してpythonで書いている記事をぐぐる
  - https://qiita.com/Kiri8128/items/cbaa021dbcb07b5fdb92

## 提出コード
```python
N = int(input())  # 頂点数
X = [[] for i in range(N)]  # 辺リスト
E = []
for i in range(N - 1):  # 1-index を0-index に直して格納
    x, y = map(int, input().split())
    E.append((x - 1, y - 1))
    X[x - 1].append(y - 1)
    X[y - 1].append(x - 1)

# bfs トポロジカルソート
from collections import deque

P = [-1] * N  # P[i] はiの親。iが根なら-1
Q = deque([0])  # queue。根にするやつを最初に追加
R = []  # トポロジカルソート
while Q:
    i = deque.popleft(Q)
    R.append(i)
    for a in X[i]:
        if a == P[i]:
            continue
        P[a] = i
        X[a].remove(i)  # 子への頂点のみを持つ
        deque.append(Q, a)

q = int(input())
dp = [0] * N
dp_tmp = [0] * N
for i in range(q):
    t, e, x = map(int, input().split())
    a, b = E[e - 1]
    if t == 2:
        b, a = E[e - 1]
    if P[a] != b:  # aが親になる
        dp[0] += x
        # dp_tmp[a] -= x
        dp[b] -= x
    else:
        dp[a] += x
        # dp_tmp[a] -= x
ans = [0] * N
for i in R:  # トポロジカルソートで親から
    for j in X[i]:
        dp[j] += dp[i]
    ans[i] = dp[i] + dp_tmp[i]
for i in range(N):
    print(ans[i])
```

## 感想
- 30min で記事を理解して実装できるか、というゲームだった
  - 理解するしたこと
    - トポロジカルソートとは、有向非循環グラフにおいて順序順にソートしたもの(木は必ず非循環グラフ)
    - トポロジカルソートすることで、適当にrootをとった時に、それぞれの辺にたいしてどっちがrootよりかが分かる。
    - 実装としては
      - 根にする頂点を最初にdequeに入れてbfsをする
      - 各頂点に対して親をリストでもつ
- Kiriさんのコードだと葉ノードからみているのに親ノードからプログラムを組んでいてバグを出していた。それがなければほとんどそのままでACだったのに、という後悔が残る。。




# F - Close Group
## 概要
問題文を読んだが理解できなかったので諦めた



