---
title: "AGC033 A Darker and Darker解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python",]
published: true
---

# URL
https://atcoder.jp/contests/agc033/tasks/agc033_a

# 制約
$$ 1<= H,W<= 1000$$

# 問題概要
- H*Wのgridがあり、いくつかのマスは黒色に塗られている。
- 以下の操作を繰り返す時、全てますが黒くなるのは何回の操作を行った時か？
- 操作：全ての黒のマスについて上下左右の隣接しているマスを黒くする

# 提出コード
```python
from collections import deque

h, w = map(int, input().split())
grid = [[0] * w for _ in range(h)]
q = deque()
black_cnt = 0
for i in range(h):
    s = input()
    for j in range(w):
        if s[j] == "#":
            grid[i][j] = 1
            q.append((i, j))
            black_cnt += 1
ans = 0
while black_cnt < h * w:
    next_q = deque()
    ans += 1
    while q:

        pos = q.popleft()
        for mov in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            y, x = pos[0] + mov[0], pos[1] + mov[1]
            if 0 <= x and x < w and 0 <= y and y < h and grid[y][x] == 0:
                grid[y][x] = 1
                next_q.append((y, x))
                black_cnt += 1
    q = next_q
    #print(ans, q, black_cnt)
print(ans)

```

# 考察
- 愚直に考えると、全ての黒マスに対して、そこから全マスへの距離を計算して、その各マスごとの最小値の最大値を取れば良いが、計算量はO((HW)^2)でTLEする
- 探索する箇所をO(HW)にする必要がある。
- 全部queueに入れて１ターンにそのqueueに入っているマス全てに対して上下左右の4マスをみると解けそう

# 実装方針
- 番兵をおけない問題なので、gridの境界条件で弾く処理を忘れずにかく
  - 0 <= x and x < w and 0 <= y and y < h

# 次回への反省
- 考察に時間をかけてしまったが、O(HW)なら通るので、全てのマスを一回しか通らないような探索の仕方を実装すればいいだけ、と気付いたらすぐだったので、計算量を意識して解放を考える

# 参考
