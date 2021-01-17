---
title: "解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python",]
published: true
---

# URL
https://atcoder.jp/contests/keyence2021/tasks/keyence2021_b

# 制約
$$ 1<=K <= N <=3*10^{5} $$
$$ 0<= a[i]<=N $$

# 問題概要
- 長さNの数列があり、値はa[i]
- これらの値をKのクラスに分割する。ただし、１つも値が割り当てられないクラスがあってもいい。
- Kのクラスに対して、そのクラスが持つ整数xはそのクラスに割り当てられた値に一致しない最小の非負整数(0,1,3,5なら2,で2,3なら0)
- クラスの整数の総和の最大値を求めよ

# 提出コード
```python
n, k = map(int, input().split())
a = list(map(int, input().split()))
hist = [0] * n
for i in a:
    hist[i] += 1
ans = 0
val = [0] * n
val[0] = min(k, hist[0])
ans += val[0]
for i in range(1, n):
    val[i] = min(k, val[i - 1], hist[i])
    ans += val[i]
print(ans)


```

# 考察
- 性質を考えると、最大値をとるために、0はできるだけ分散、1以降の数字は0が入っている箇所に連番で置く必要がある。
- 連番(0,1,2,3..)のセットをどれだけの長さをそれぞれ何個作れることができるかを知りたい
- どうやって3*10^5に対して計算するか？
  - ヒストグラムを前処理で計算する。１つ前の長さの連番が何個作れたか、から次を計算する。

# 実装方針
- val[i] = 値iに対してそこまで連番になるクラスがいくつあるか？　と定義する。 
- a < N より、それでループ回して更新する
- val[i+1] = min(val[i],a.count[i+1],k) 
- count を高速に計算するため、長さNの配列を持ってヒストグラムをあらかじめ作っておく
- ans += val[i]とする
# 次回への反省
- 

# 参考
