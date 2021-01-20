---
title: "全国統一プログラミング王予選 - Different Strokes解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","ゲーム"]
published: true
---

# URL
https://atcoder.jp/contests/nikkei2019-qual/tasks/nikkei2019_qual_c

# 問題概要
- a,b 二値を持つN この整数列がある。
- 二人はa+b が大きい順から交互にとっていく。
- 最終的な差はいくらか
（説明が恐ろしく下手なので問題文を見てください）

# 制約
$$ N<=10^{5} $$
# 提出コード
```python
n = int(input())
a_plus_b = [0] * n
a_minus_b = [0] * n
ans = 0
for i in range(n):
    a, b = map(int, input().split())
    a_minus_b[i] = a - b
    a_plus_b[i] = a + b
    ans += a
a_plus_b, a_minus_b = zip(*sorted(zip(a_plus_b, a_minus_b)))
for i in range(n):
    if i % 2:
        ans -= a_plus_b[n - 1 - i]
print(ans)

```

# 考察
- 単純にa+bでソートして降順にとっていくだけ。
- O(NlogN)は間に合うので実装

# 実装方針
- 先に全部A がとった時からB がとった時の差分を計算
# 次回への反省
- 先に全部A がとった時からB がとった時の差分を計算したけど、普通にA,Bをそれぞれ交互にとっておく操作を実装した方が見やすかった。

# 参考
