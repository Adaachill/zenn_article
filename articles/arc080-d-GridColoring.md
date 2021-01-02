---
title: "arc080-d-GridColoring メモ[python]"
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
  - 左上からコの字型にaiだけマスをとっていけば良いQ

# 実装方針
- 0-indec で見ると偶数行は左から(0から）、奇数行は右から(w-1から）みていくことになる
- 変数として、現在塗る色のindexと開始場所を持つ
- １行ずつ処理して出力する

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
  にしていてバグらせた。注意---
title: ""
emoji: "👏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---
