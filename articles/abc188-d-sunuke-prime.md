---
title: "ABC188 D - Snuke Prime解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","座圧","累積和"]
published: true
---

# URL
https://atcoder.jp/contests/abc188/tasks/abc188_d
# 問題概要
- N このサービスがあり、それぞれai日からbi 日まで利用する。そして単品契約の場合1日あたりci かかる
- 全てのサービスを使えるプライム会員は1日あたりC で契約できる
- 適切に契約するときの全体のコストの最小値を求めよ
- 解除料金、初期料金はない

# 制約
$$ 1<=N <= 2*10^{5}$$
$$ 1 <= ai <= bi <= 10^{9}$$

# 提出コード
```python
import bisect

n, C = map(int, input().split())
a, b, c = [0] * n, [0] * n, [0] * n
zaatu_day = []
for i in range(n):
    x, y, z = map(int, input().split())
    a[i] = x - 1
    b[i] = y
    c[i] = z
    zaatu_day.append(a[i])
    zaatu_day.append(b[i])
zaatu_day = list(set(zaatu_day))  # ソートした上でuniqueにする
zaatu_day.sort()

# print(zaatu_day)
dif = [0] * len(zaatu_day)
accum = [0] * len(zaatu_day)
for i in range(n):
    start_idx = bisect.bisect_left(zaatu_day, a[i])
    dif[start_idx] += c[i]
    end_idx = bisect.bisect_left(zaatu_day, b[i])
    dif[end_idx] -= c[i]
ans = 0
for i in range(len(zaatu_day)):
    accum[i] = accum[i - 1] + dif[i]
    if i < len(zaatu_day) - 1:
        ans += (zaatu_day[i + 1] - zaatu_day[i]) * min(C, accum[i])
print(ans)

```

# 考察
- a,b の値が小さければ単純に単品契約した際のそれぞれの日のコストをリストなどで管理してmin(C,sum(その日のコスト))を足していけばいい
- 単純にこれをやるとそれぞれの日にコストを埋めるところが重くて、計算量はO(sum(bi-ai) + 10**9)かかるのでだめ
- imos 法的に開始点+ciと終了時-ciのを値を入れて、累積和をとることでそれぞれの日のコストをとることを考えると、前処理にO(N),累積和の計算にO(b)で計算量を削減できる
- ただし、今回のケースでは、a,b<10**9のため、そのままでは全体を累積和とることができない
- そのため、座標圧縮と呼ばれるテクニックを使う
  - 取りうる値をソートしてindex のみを保持するという手法
  - この手法を使うと長さは高々O(N)になるので計算できる。


