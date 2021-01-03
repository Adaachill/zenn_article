---
title: "木DPの練習1　ABC036 D - 塗り絵 メモ[python]"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","数え上げ","木DP"]
published: true
---

ABC187Eで木DPが出て実装でつまづいたので練習する

# URL
https://atcoder.jp/contests/abc036/tasks/abc036_d

# 問題概要
全頂点が連結で、辺がN-1個あるN頂点のグラフが与えられる。
それぞれの頂点を隣接する頂点が両方黒色にならないように白黒２種類の色で塗り分ける時塗り方は何通りあるか10**9+7で割ったあまりを求める

#　制約
N<=10**5

# 提出コード
```python
n = int(input())
node = [[] for _ in range(n)]  # 隣接リスト
e = []
for i in range(n - 1):
    x, y = map(int, input().split())
    e.append((x - 1, y - 1))
    node[x - 1].append(y - 1)
    node[y - 1].append(x - 1)

from collections import deque

mod = 10 ** 9 + 7
p = [-1] * n  # p[i] はi の親。根なら-1
q = deque([0])  # 根はどこでも良い
r = []  # トポロジカルソート.根から浅い順番にソートされて入っている

dp_black = [1] * n
dp_white = [1] * n

# dfs ：　トポロジカルソートと葉ノードのdp初期化
while q:
    i = deque.popleft(q)
    r.append(i)
    # print(node[i])
    for a in node[i]:  # i の子node を探索する
        if p[a] != -1:
            continue
        p[a] = i
        node[a].remove(i)  # 子への頂点のみを持つ
        deque.append(q, a)

for i in r[::-1]:  # 葉ノードから順番にdpを埋める
    for j in node[i]:  # 子ノードからの値を足していく
        dp_black[i] *= dp_white[j]
        dp_white[i] *= dp_white[j] + dp_black[j]
        dp_white[i] %= mod
        dp_black[i] %= mod
print((dp_white[0] + dp_black[0])%mod)

```

# 考察
- 入力の性質：全頂点が連結で、辺がN-1個あるN頂点のグラフ = このグラフは木である
  - 木の定義として連結かつ、ループがない
  - ループでない１つの辺によって１つ連結成分を増やすことができるので辺がN-1しかない時、ループはないことになる
- 操作の言い換え：言い換えるのは難しいので答えから逆算することを考える
  - 状態数はNaive に全部を考えるとそれぞれの頂点を2種類に分けるのでO(2**N)で、その状態が条件を満たすかの判定にO(N)かかるので状態数をまとめる必要がある
  - グラフは木でループがないため、適当に根を指定した時に、頂点i が白(黒)である場合の数はその部分木の積として表すことができる。
    - 漸化式に落とすと jはi の　子ノード集合 として(積和の数式をPiとかくと )
      - dp_black[i] = Pi(dp_white[j])
      - dp_white[i] = Pi(dp_white[j] + dp_black[j])
    - 答えとしては葉ノードから順番にdp を埋めて根のwhite,black の値の和となる
    - 初期値は全ノードはblakc,white共に1になる

# 次回への反省
- 考察を詰めた後、実装してACするまでに30min かかったので反省
  - 漸化式が間違っていた
    - dp_black,dp_whiteはそれぞれ子ノードの積和（総乗）として表すことができる（総和かと思っていたが独立にとることができるので積になる）
    - 漸化式が間違っていたので初期値も変更(全ノード1でいい)
  - modをとるのを忘れていた