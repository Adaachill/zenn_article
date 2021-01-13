---
title: "ABC181 E Transformable Teacher解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python",]
published: true
---

# URL
https://atcoder.jp/contests/abc181/tasks/abc181_e
# 制約
$$ 1 <= N,M <= 2*10^{5} $$
1 <= Hi,Wi <= 10^{9} $$

# 問題概要
- 数直線上にN点が存在する。
- M回のクエリが与えられ、各クエリでは数直線上の点が一点与えられる。
- その点を加えたN+1点を対象にして
- 小さい方から二点ずつペアを作った時のペアの数値の差の和を最小にしたい

# 提出コード
```python
import bisect

n, m = map(int, input().split())
h = list(map(int, input().split()))
w = list(map(int, input().split()))
h.sort()
acc = [0] * (n + 1)
for i in range(n):
    acc[i + 1] = acc[i] + h[i] * pow(-1, i + 1)
INF = 10 ** 15
ans = INF
for i in range(m):
    idx = bisect.bisect_left(h, w[i])
    # print(w[i], idx, h)
    ans = min(ans, w[i] * pow(-1, idx + 1) + 2 * acc[idx] - acc[-1])
print(ans)

```

# 考察
- 愚直にどの点を加えるかを固定して毎回計算すると、O(NM)となってTLEする。
- 明らかにM点から選ぶ点が近い時に計算式の大部分は同じになりそうなので前計算して点を固定した時の計算量を削減することを考える
  - acc[i] = acc[i-1] - h[i]*((-1)**i)　という累積和を考えると
  - w[j]がh[i-1] < w[j] < h[i] の時（h[-1] = 0とする）、
  - 求めたい式は w[j]*pow(-1,i+1) + acc[:i] - acc[i:] となる
  - 累積和の計算はO(N)で累積和を計算した上でこの計算はO(1)になるので全体の計算量は二分探索の計算量をかけて　O(NlogN+MlogN)　となる


# 実装方針
- h[i]は入力時にソートする
- 二分探索はbisectで行う。



# 参考
https://drken1215.hatenablog.com/entry/2020/11/02/021500