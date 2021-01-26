---
title: "ABC176 E - Bomber解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","adhoc"]
published: false
---

# URL
https://atcoder.jp/contests/abc176/tasks/abc176_e

# 制約
$$ 1<=H,W <= 3*10^{5}$$
$$ 1<= M<=min(H*W,3*10^{5}) $$
(hi,wi) != (hj,wj)
# 問題概要
- H*Wのグリッドがあり、Mこのマスにターゲットがある。
- １つのマスを選び、そのマスと同じ行、または列にあるターゲットの数を最大化したい。何個になるか？

# 提出コード
```python
#!/usr/bin/env python3
import bisect

H, W, M = map(int, input().split())
row = [0] * (1 + H)
col = [0] * (1 + W)
match = []
for i in range(M):
    h, w = map(int, input().split())
    h -= 1
    w -= 1
    row[h] += 1
    col[w] += 1
    match.append(h * (10 ** 6) + w)
match.sort()
row, idx_r = zip(*sorted(zip(row, range(1 + H))))
col, idx_c = zip(*sorted(zip(col, range(1 + W))))
max_r = []
max_c = []
i = 0
while True:
    if row[-i - 1] == row[-1]:  # maxならそのindexをmax_r に追加する
        max_r.append(idx_r[H - i])
    else:
        break
    i += 1
i = 0
while True:
    if col[-i - 1] == col[-1]:
        max_c.append(idx_c[W - i])
    else:
        break
    i += 1
# print(max_c, max_r, col, row, idx_c, idx_r)
match_flag = 1
for i in max_r:
    for j in max_c:
        if bisect.bisect_left(match, i * (10 ** 6) + j) == bisect.bisect_right(
            match, i * (10 ** 6) + j
        ):
            match_flag = 0
            # print(match, i * (10 ** 6) + j, i, j)
            break
print(col[-1] + row[-1] - match_flag)
```

# 考察
- 単純に行、列ごとにターゲットの数を数えて、行のmax + 列のmax をすれば良い
- もし、選んだ行、列の交点にターゲットがあれば-1する

# 実装方針
- 選んだ行、列の交点にターゲットがあルカの判定は3*10**5+1をかけたりして1次元の数字に落とす
- max になる行、列のindex はzip のsortとかで計算する。

# 次回への反省
- 計算量は？　Mが対角線上に並んでいるときなどがヤバくてO(M^2)かかる？　とりあえず投げたらACした。。
- 解説を読むと(h,w)の組でsetに入れて探索している

# 参考
https://atcoder.jp/contests/abc176/editorial/66