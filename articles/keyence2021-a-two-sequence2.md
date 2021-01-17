---
title: "Keyence2021 A - Two Sequences 2解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","最大値","順次更新"]
published: true
---

# URL
https://atcoder.jp/contests/keyence2021/tasks/keyence2021_a

# 制約
$$ 1 <=N <= 2*10-{5}$$
$$ 1<=a[i],b[i] <= 10^{9}$$

# 問題概要
- 長さNの数列a,bがある
- 1<=i<=Nとなるi それぞれについて、Ci= max(a[j]*b[k])(1<=j<=k<=i)で定義されるCiを求めよ
# 提出コード
```python
n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
a_max = 0
c = 0
for i in range(n):
    a_max = max(a_max, a[i])
    c = max(c, a_max * b[i])
    print(c)


```

# 考察
- 前の計算結果をうまく利用して計算量を落とす問題
- 素直にやるとそれぞれの操作でO(N^2)とかかかり全体でO(N^3)
- C[i+1]でC[i]で考慮する値から追加される値を考えると、b[i+1]*max(a[:i+2]) だけであることがわかる
- つまり、a[i]の現在までのmaxを変数としてもち、順次更新して、C[i+1]=max(C[i],b[i+1]*a_max)で答えがもとまる

# 実装方針
- 変数はC とa_maxだけでいい
- Nでループを回して順次更新していく

# 次回への反省
- 最初i<=jという条件を見落としていたので注意

# 参考
