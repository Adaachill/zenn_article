---
title: "ABC132 D - Blue and Red Balls解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","combination","数学"]
published: false
---

# URL
https://atcoder.jp/contests/abc132/tasks/abc132_d

# 制約
$$1<=K<=N<=2000$$

# 問題概要
- Kの青いボールとN-Kの赤のボールがある。
- 青いボールが連続している箇所がi箇所になるような並べ方を1<=i<=Kとなるi についてそれぞれに10**9+7で割ったあまりを求めよ

# 制約
$$ 1<=K<=N<=2*10**5 $$

# 提出コード
```python

```

# 考察
- 青色の連続したボールをまず１つのボールとしてカウントすると、仕切りの入れ方を考える問題と考えられ、n-k+iCi 
- 次にK 個をiに分割する方法はk-1Ci-1
- n-k+iCi * k-1Ci-1= f(i) とすると、
$$ f(i+1) =f(i)*(n-k+i+1)/(i+1)*i*(k-1-i) $$
となるので順番に計算していく