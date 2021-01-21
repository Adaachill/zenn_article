---
title: "ABC185 F - Range Xor Query解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","巡回"]
published: true
---

# URL
https://atcoder.jp/contests/arc109/tasks/arc109_c
# 制約
$$ 1<= N,K<= 100 $$
s はRPSのみからなる長さn の文字列

# 問題概要
- 2^k 人でジャンケントーナメントをする。
- ヒトは0-indexで並んでいて index i の人は毎回s[i%n]で表される手を出す
- あいこの場合はindexが小さい方がかつ。
- 大会勝者の出す手を求めよ

# 提出コード
```python
n, k = map(int, input().split())
s = input()
for i in range(k):
    s = s + s  # あらかじめ２つつなげる
    next_s = []
    for j in range(n):
        if s[2 * j] == s[2 * j + 1]:
            next_s.append(s[2 * j])
        elif s[2 * j] in "RS" and s[2 * j + 1] in "RS":
            next_s.append("R")
        elif s[2 * j] in "PR" and s[2 * j + 1] in "PR":
            next_s.append("P")
        elif s[2 * j] in "SP" and s[2 * j + 1] in "SP":
            next_s.append("S")
        else:
            raise Exception
    s = "".join(next_s)
print(next_s[0])

```

# 考察
- 繰り返し二乗法みたいな感じで半分ずつとかに割りたい
  - トーナメント表を直接に考えると難しい
  - 一回戦の左からi番目の人がs[i] を出すように、n回戦での左からi番目の人がs'[i] となるようなs' を順番に構成すれば良い。

# 実装方針
- s を更新していく
  - s[2*i]とs[2*i+1]との勝者がs'[i]となる
- k 回繰り返す

# 次回への反省
- シンプルな問題設定だが、二日くらい置いて考えた後自力AC
- 2^k と考える値が大きいので繰り返し二乗法のように部分に分割して考えるという発想は初期からあったが、トーナメント表を分割するという発想が先に来て、何回戦か？で再帰的に分割するという発想に至るまでに時間がかかった。
- 繰り返し二乗法っぽい問題は同じ処理になるように新しく変数とかをおけないか考える。

# 参考
https://qiita.com/Kept1994/items/ea91c057b0e552323da3 ダブリングの解説。DP的なテーブルを作って、2^i 後の遷移をその2^(i-1)での状態の遷移から計算する
