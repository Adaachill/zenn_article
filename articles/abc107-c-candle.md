---
title: "ABC107 C Candle解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","幾何","構築"]
published: true
---

# URL
https://atcoder.jp/contests/abc107/tasks/arc101_a

# 問題概要
- 正負の方向に伸びる数直線上にN個の点がある
- 座標0からKこの点を通過するのに必要な移動距離を求めよ

# 提出コード
```python
import bisect

n, k = map(int, input().split())
x = list(map(int, input().split()))
x.sort()
if 0 in x:
    k -= 1
idx = bisect.bisect_left(x, 0)
pos = [0]
neg = [0]
# 先頭に追加 0　のやり方簡単なのがない気がするので空リストに追加していく

pos_l = 1
neg_l = 1
for i in range(n):
    if x[i] < 0:
        neg.append(-x[i])
        neg_l += 1
    elif x[i] > 0:
        pos.append(x[i])
        pos_l += 1
neg.sort()  # ここを忘れていた。
ans = 10 ** 9  # INF
idx = -1
for i in range(k + 1):
    if i < neg_l and k - i < pos_l:
        ans = min(ans, pos[k - i] + neg[i] + min(neg[i], pos[k - i]))

print(ans)

```

# 考察
- 結構単純な問題設定。座標0から始まるのでXiが正負どちらかに移動する
- Xi をソートして正負のどちらもやればいい。
  - K本は固定なので、負の方向にi 本(0<=i<=K)とるって決める
  - 正負どっちを先にとるかは、正負の移動距離が小さい方を先に進む（先に進む方が*2になるから）

# 実装方針
- N この座標を正負に分けた上でソートする(pos,neg,両方正絶対値を持つ、index 0 は0にする)
- 負の方向にi 本(0<=i<=K)とるとした時に移動距離は pos[K-i+1] + neg[i] + min(pos[K-i+1],neg[i])


# メモ
- 0が入っている時の処理や正負それぞれをソートして格納する方法などいくつかミスのポイントがあった

# 参考
