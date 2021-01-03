---
title: "ARC080-D-GridColoring 解説[python]"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","構築"]
published: true
---

# 問題概要
H*Wのグリッドがあり、N色で塗り分ける。
次の条件を満たす塗り分け方を１つ出力せよ。
- 色i のマスはちょうどai存在する
- 各色についてその色のマスは上下左右に連結
H,W<=100

# 考察
- 構築系の問題なので、考えやすいパターンで条件を満たすものを考える
  - 上下左右に連結だが、直線的に配置していると考えると楽
  - 左上から一筆書きになるように蛇型で色i はaiだけマスをとっていけば良い

# 実装方針
- 0-index で見ると偶数行は左から(0から）、奇数行は右から(w-1から）みていくことになる
- 変数として、現在塗る色のindexと開始場所を持つ
- １行ずつ処理して出力する

# 提出コード
```python
h, w = map(int, input().split())
n = int(input())
a = list(map(int, input().split()))
now_idx = 0
paint_num = a[0]
ans = [[-1] * w for _ in range(h)]
for i in range(h):
    for j in range(w):
        if paint_num == 0:
            now_idx += 1
            paint_num = a[now_idx]
        if i % 2:
            ans[i][w - 1 - j] = now_idx
        else:
            ans[i][j] = now_idx
        paint_num -= 1
# print(ans)
for i in range(h):
    for j in range(w):
        print(ans[i][j] + 1, end=" ")
    print()

```

# 次回解く時のメモ
- 最初に計算量に着目してO(HW)が通ることに注目する
- 不要な変数をとって実装が混乱したので変数の選択は注意する
