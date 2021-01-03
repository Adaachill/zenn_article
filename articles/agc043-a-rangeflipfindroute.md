---
title: "緑dif埋め　AGC043A - Range Flip Find Route解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","数え上げ","木DP"]
published: true
---


# URL
https://atcoder.jp/contests/agc043/tasks/agc043_a
# 問題概要
- h*w のgridがあり、それぞれのgridは白か黒で塗られている
- 右端から左下のマスまで常に白のマスのみをとおって最短距離で移動できるようなパスがあるようにgridを塗り替えるように以下の操作を何回か行う。操作の最小回数を求めよ
- 操作:gridに並行な四角形を選択し、その範囲のマスを全て色を反転させる


#　制約
2 <= h,w <= 100

# 提出コード
```python
#!/usr/bin/env python3
h, w = map(int, input().split())
grid = [["#"] * (w + 2) for _ in range(h + 2)]
for i in range(h):
    s = input()
    for j in range(w):
        grid[i + 1][j + 1] = s[j]

INF = 10 ** 6
dp = [[INF] * (w + 2) for _ in range(h + 2)]
dp[1][1] = int(grid[1][1] == "#")  # 黒なら1、else 0

for i in range(1, h + 1):
    for j in range(1, w + 1):
        if i == 1 and j == 1:
            continue
        val_bottom = dp[i - 1][j] + (grid[i - 1][j] == "." and grid[i][j] == "#")
        val_left = dp[i][j - 1] + (grid[i][j - 1] == "." and grid[i][j] == "#")
        dp[i][j] = min(val_bottom, val_left)
print(dp[h][w])

```

# 考察
- 入力の性質：2値のいずれかを持つgrid,特に他の制約なし
- naive に全探索的な方法を考えると,左上から右端までの最短経路になりうるパスはh+wCw 通りある。
  - それぞれのパスに対し、初期状態で黒が連続する箇所を１箇所ずつ反転させればそのパスに対しては最適 O(h+w)
  - ただしパスの数がmax は200C100になるのでTLE

- 状態数の削減方法
  - 以上の考察から結局あるパスが決まった時、操作回数はpath中の黒が連続している（折れ曲がっていても縦横につながっているな可）箇所の個数になるとわかる
  - 上記の性質を満たすようなpathを探したいが、TLEを防ぐために状態数を削減したい ダイクストラ(dp)ができそう
- dp 
  - 定義：最短経路を持つpathのみ対象なことから各マスに対して、
    - ```dp[i][j] :=　(1,1)から(i,j)までの最短path中で黒が連続している箇所の個数が最小になるpathの連続箇所の個数```　
    となる
  - 初期値：INF初期化,```dp[0][0] = (grid[0][0] == 白)```
  - 漸化式：```dp[i][j] = min(dp[i][j-1]+black,dp[i-1][j]+black)```
    - black は１つ前の状態が白でdp[i][j]が黒なら1 他は0をとる 

# 次回への反省
- pythonの文字列のgridを与えられる時の注意
 - ```grid = [["#"*w] for _ in range(h)]``` だとだめ。文字列に対しては代入ができない
 - ```grid = [["#"] * (w) for _ in range(h)]``` のように１文字づつリストにする
- 番兵をとったものの、
  - ```if i == 1 and j == 1:continue``` を忘れていて番兵の箇所によって初期値が書き換えられるというバグを生んでいた
   - 番兵置くときは教会条件は考えなくて済むというメリットはあるが初期化や変数の値には注意

