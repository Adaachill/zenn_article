---
title: "DDDC2020 予選 C - Strawberry Cakes解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","構築"]
published: true
---

# URL
https://atcoder.jp/contests/ddcc2020-qual/tasks/ddcc2020_qual_c

# 問題概要
- h*wの格子状のケーキがあり、その中のK のマスにはイチゴが入っている
- gridと並行にケーキをKこのピースに分ける
- イチゴが全てのピースにひとつづつ含まれるようなものを出力せよ

# 提出コード
```python
#!/usr/bin/env python3
h, w, k = map(int, input().split())
s = []
for i in range(h):
    s.append(input())
out = [[0] * w for _ in range(h)]
idx = 0
for i in range(h):
    first_pos = -1
    first_idx = 0
    for j in range(w):
        if s[i][j] == "#":
            idx += 1
            out[i][j] = idx
            if first_pos == -1:
                first_pos = j
                first_idx = idx
        else:
            if first_pos != -1:
                out[i][j] = idx
    for j in range(first_pos):
        out[i][j] = first_idx
for i in range(h):
    for j in range(w):
        if i == 0:
            continue
        else:
            if out[i][j] == 0:
                out[i][j] = out[i - 1][j]
for i in range(h - 1, -1, -1):
    for j in range(w):
        if i == h - 1:
            continue
        else:
            if out[i][j] == 0:
                out[i][j] = out[i + 1][j]
for i in range(h):
    print(*out[i])
```

# 考察
- わからなかったので解説記事を読んだ。
- 上下左右にgridを走査して、もし# だったらidx を増やし、#　でない場合はその前のgridと同じidx にする

# 実装方針
- 上下左右それぞれの走査に対して二重のfor 文を回す
- 出力用の配列を確保してそこにidx を入れる
- 現在すでにidx が確定しているかを0 か否かで明示的に持つ

# メモ
- コンテストに参加した時に解けなかった問題でその時に解説を読んだ記憶がうっすらと蘇ってくる
- 端から順番に長方形を作っていけば良い。
- できるだけ小さくか、大きく取る。大きく撮ればちょうどKに分割できる
- どうやってみていくかというと、端から順番に操作する
- 構築は発想ゲー

# 参考
maspy さんの解説記事(https://maspypy.com/atcoder-%E5%8F%82%E5%8A%A0%E6%84%9F%E6%83%B3-2019-11-23ddcc2020%E4%BA%88%E9%81%B8)
- わかりやすすぎる。複雑に実装を考えていたのが上下左右に伸ばしていけばいいのか！となった。
- 自分でこれを思いつくのは辛い