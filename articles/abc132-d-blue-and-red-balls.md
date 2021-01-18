---
title: "ABC132 D - Blue and Red Balls解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","combination","数学"]
published: true
---

# URL
https://atcoder.jp/contests/abc132/tasks/abc132_d

# 制約
$$1<=K<=N<=2000$$

# 問題概要
- Kの青いボールとN-Kの赤のボールがある。
- 青いボールが連続している箇所がi箇所になるような並べ方を1<=i<=Kとなるi についてそれぞれに10**9+7で割ったあまりを求めよ

# 制約
$$ 1<=K<=N<=2*10^{3} $$

# 提出コード
```python
n, k = map(int, input().split())
mod = 10 ** 9 + 7
f = [0] * (k + 1)
f[1] = n - k + 1
print(f[1])
for i in range(1, k):
    f[i + 1] = f[i] * (n - k + 1 - i) * (k - i) * pow(i, -1, mod) * pow(i + 1, -1, mod)
    f[i + 1] = f[i + 1] % mod
    print(f[i + 1])

```

# 考察
- 青色の連続したボールをまず１つのボールとしてカウントすると、仕切りの入れ方を考える問題と考えられ、n-k+1Ci 
- 次に0を許さずにK 個をiに分割する方法はk-1Ci-1
- n-k+1Ci * k-1Ci-1= f(i) とすると、
$$ f(i+1) =f(i)*(n-k+1-i)*(k-i)/(i+1) *i   $$
となるので順番に計算していく

# 次回への反省
- WAが出た理由がわからなかったのだが、

$$ f[i + 1] = f[i] * (n - k + 1 - i) * (k - i) // (i*(i+1)) $$ 

として更新していた。
modで順次割るときは割り算は逐一modivをかける形にする必要がある。