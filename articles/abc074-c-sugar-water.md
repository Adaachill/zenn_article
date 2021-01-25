---
title: "ABC074 C - Sugar Water解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","二分探索"]
published: true
---

# URL
https://atcoder.jp/contests/abc074/tasks/arc083_a

# 制約
$$ 1<= A < B  <= 30 $$
$$ 1<= C < D  <= 30 $$
$$ 1<= E <= 100 $$
$$ 100A <= F <= 3000 $$


# 問題概要
- 水と砂糖を混ぜてできるだけ純度の高い砂糖水を作りたい
- 操作としては100A,100Bの水を入れる、砂糖C,Dg を入れるの4種類の操作がある
- 水100あたり砂糖Eまで溶けるとする
- 合計はF以下である。

# 提出コード
```python
import bisect

a, b, c, d, e, f = map(int, input().split())
sugar_max = f * e / (100 + e)
sugar = []
for i in range(0, int(1 + sugar_max // c)):
    for j in range(0, int(1 + sugar_max // d)):
        if i * c + j * d <= sugar_max:
            sugar.append(i * c + j * d)
sugar = list(set(sugar))
water = []
for i in range(0, 1 + f // (100 * a)):
    for j in range(0, 1 + f // (100 * b)):
        if i * a * 100 + j * b * 100 <= f:
            water.append(i * a * 100 + j * b * 100)
water = sorted(list(set(water)))
cur_max = 0
ans = (0, 0)
# print(sugar, water)
for i in sugar:
    if i == 0:
        continue
    # 二分探索して見つける。F 以下であるなら比較
    idx = bisect.bisect_left(water, i * 100 / e)
    if idx >= len(water):
        continue
    # print(i, i + water[idx], idx, i * 100 / e)
    if i + water[idx] <= f:
        if cur_max < i / (i + water[idx]):
            cur_max = i / (i + water[idx])
            ans = (i, water[idx])
if ans[0] == 0:
    print(a * 100, 0)
else:
    print(ans[0] + ans[1], ans[0])

```

# 考察
- 値が小さいので全探索的な感じでナイーブにいけそう
- 合計がF なので、砂糖の量はF*E/(100+E)以下になる。
  - C,Dの組み合わせで砂糖の量がF*E/(100+E)以下になる　値を全探索して、それぞれの値に対して水の量を調整したい
  - 水の量を調整する時はあらかじめ作りうる水の量を列挙して二分探索する

# 実装方針
- 取りうる水、砂糖の量の列挙は余裕を持っておいて良い
- 水の量だけソートしておく

# 次回への反省
- 砂糖の量が0の時に適当に水を出力しないといけないというところでWA
- 水の量が1/100で与えられていることに気づかず実装やり直し。

# 参考
