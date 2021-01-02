---
title: "AGC011 A AirportBus メモ[python]"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python"]
published: true
---

# 問題概要
N人の人がそれぞれTi 時刻に空港に到着する
それぞれの待ち時間がK 以下になるようにピックアップのバスを用意する必要がある。
１つのバスにはC 人まで乗ることができるときバスは何個必要か

# 考察
- C人の人がたまるか、先頭の人がK待つことになるかまで貪欲に考えていけば良い
  - Ti をソートして前から処理する
  - 現在空港にいる先頭の人の時刻、次に処理する人にidx,現在の乗客の人数を変数として持つ

# 実装方針
- while を使って全体を見るようにする

# 提出コード
```python
n, c, k = map(int, input().split())
t = [int(input()) for _ in range(n)]
t.sort()
ans = 1
lead_time = t[0]
idx = 1
num_passenger = 1
while idx < n:
    if lead_time + k < t[idx] or num_passenger + 1 > c:
        lead_time = t[idx]
        num_passenger = 1
        ans += 1
    else:
        num_passenger += 1
    idx += 1

print(ans)
```

# メモ
- 最初　`num_passenger + 1 > c` を`num_passenger  > c`
  にしていてバグらせた。注意