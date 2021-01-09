---
title: "ARC100  C - Linear Approximation解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","median","知識"]
published: true
---

# URL
https://atcoder.jp/contests/arc100/tasks/arc100_a

# 問題概要
- 整数列Aがあり、abs(a[i]-x)の最小値を求めよ


# 制約
N<=2*10**5

# 提出コード
```python
```

# 考察
- Aの値をa[i] = a[i]-i とすればabs(a[i]-x)の最小値　を求める問題に言い換えられる
- 絶対値の最小値：平均になりそう WA
- 解説をみると絶対値の最小値　はmedian になるらしい
  - 証明：現在の値がmedian でない時、A中の現在の値より大きい値の数をx、小さい値の数をy とする。現在の値を+p（median をまたがない範囲で）移動させる時、絶対値の総和は-x*p + y*p となる
  - よってその値より大きい数と小さい数の個数が同じであるような値の時最小値となるためmedianとなる
  - Aの長さが偶数の時はa[n//2],a[n//2-1]の間（開区間）ならば任意の値を取れる

# 実装方針
- a[i] = a[i]-i としてソートする。
- median を求める



# 次回への反省
- average になるという固定観念から抜け出せなかった
 - 手元でいろいろなケースを作って試すこと

# 参考
- https://drken1215.hatenablog.com/entry/2019/06/15/114700