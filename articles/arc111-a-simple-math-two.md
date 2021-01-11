---
title: "ARC111 A - Simple Math 2解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","mod","数学"]
published: true
---

# URL
https://atcoder.jp/contests/arc111/tasks/arc111_a


# 問題概要
- floor(10^n/m) をm で割ったあまりを求めよ

# 制約
$$1 <= N<=10^{18}$$
$$ 1<=M<=10^{4}$$

# 提出コード
```python
n, m = map(int, input().split())
print((pow(10, n, m * m) // m) % m)


```

# 考察
- 解説AC
- 10^n が大きいので直接持つことはできない
  - 10^n//m とmod m で同じになるような小さい値に置き換えたい
  - 10^n を m^2 で悪ると、商の部分はfloow 部分にも mod m にも関係ないので起き変えることができる。
- 発想力がなかった。（丁寧にどこが影響するか考えること）
# 実装
- python はpow という組み込み関数があるので楽できる
 - pow(n,k,m)でn^k のmod mをとることができる
