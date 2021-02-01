---
title: "AGC048 A - atcoder < S解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","貪欲"]
published: true
---

# URL
https://atcoder.jp/contests/agc048/tasks/agc048_a

# 制約
$$ 1 <= T <= 100 $$
$$ 1 <= |S| <= 1000$$

# 問題概要
- 英子文字のみからなる文字列Sが与えられ、隣あう２文字をswapするという動作を繰り返すことでatcoder < S にできるか判定し、可能な場合にはそれに必要な最小の操作回数を答える

# 提出コード
```python
t = int(input())
for i in range(t):
    s = input()
    l = len(s)
    ans = -1
    for i in range(l):
        if s[i] != "a":
            if s[i] > "t":
                ans = max(1, i - 1)
            else:
                ans = i
            break
    if s > "atcoder":
        ans = 0
    print(ans)
```

# 考察
- |S| が小さいことからO(S^2)が通ることを念頭におく
- まず、判定の方法を考えると、全要素にa 以外の文字があればそれを先頭に持ってくることができる。全要素aなら動かしても変わらないので不可能
- 最小回数を考えると、辞書順の定義的に先頭から順番に考えていく。
 - 先頭から最初にa が出てくるまでの距離を数えて答えにたす。文字列をa を除いた文字列に変換する
 - 以降は再帰的に計算できて現在の文字列を先頭からatcoderの現在の見ている文字と辞書順的に以下になるものを探してそこまでの距離をたす
 - もし、同じ文字列が先に現れたら次の再帰にいくかどうかは、その文字より小さい文字が出てくるまでの距離による。
 - 計算量はO(S^2)

# 実装方針
- 再帰的に行うので関数化
- 状態としては、現在の個数(atcoder　のどの文字と比較するか）、現在の文字列、潜ってもいい深さをもてば良い
  - 関数をans とすると求めたい答えは ans(0,S,len(S))
- ans(i,S)はatcoder のSを先頭から探査してatcoder[i]以下の文字列までの距離を返す
- if atcoder[i]　> S[j]: return j
- elif atcoder[i] == S[j] and atcoder[i] > S[k](j < k):
  - return min(k,j+ans(i+1,S',k-j))

# 次回への反省
- 結局再帰でかけなくてfor ループにして変数を更新していく形で書いた

# 参考
