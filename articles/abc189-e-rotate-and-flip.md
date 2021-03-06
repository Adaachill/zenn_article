---
title: "ARC110 C - Exoswap解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","構築"]
published: false
---


# コンテスト概要
A Slot
3つ全部同じか？
```python
s  = input()
if s[0] == s[1] and s[0] == s[2]:
  print("Won")
else:
  print("Lost")
```

B
V[i]*P[i] がXを超える時を見つける  X < 0 でプリント　ans += 1 
全探索

C 
l,r,x をいい感じに選んでmax を求める
x 以上の連続の中でsum 最大
tmp を持って、a[i] >= x ならtmp にたす
x < ならtmp = 0

D 
N の文字列 AND OR
論理積で以前の結果を持ってくる。
y0 = x0
2^(n+1)この組み合わせに対してyNがTrueになるような個数
XOR見たいに個数の問題に落としたい
一回False になったら修復できない。


F - Sugoroku2
# URL 
https://atcoder.jp/contests/abc189/tasks/abc189_f
# 制約

# 問題概要
双六で0~N までのますがある。いくつかのマスは振り出しに戻るマス。
ゴールまでルーレットを回す回数の期待値
dp と漸化式で表すことができる。
dp[0] = dp[a[0]] = dp[a[1]] = ... = dp[a[k-1]]

簡単なケースで考える。
0,1,2 で 1 だと振り出しに戻るっていうケースだと？
dp[0] = 0.5*(1+dp[0]) + 0.5 みたいな漸化式をとく
dp[0] = 2 になる。

dp をどうおくかっていうことだけが問題になる。
dp[a[i]] = 0 にして、tmp みたいな形でそれぞれの値を覚えることができればOK
あとは普通に、dp[i] = dp[i-k]/m + 1
でdp[0] = 1 にする。
普通にdpを更新するとO(N^2)になる。
K が小さいのでそれを生かして計算したい。
単純な遷移をしているから、dp[j] を計算するときにj 未満で何個k があるか？みたいな計算になりそう。



# URL
https://atcoder.jp/contests/abc189/tasks/abc189_e
# 制約
$$ 2<= N <=2*10^{5} $$
P は1,2,...,N を並び替えた数列

# 問題概要
- 数列Pの隣接する二点を任意の順番でそれぞれ一回だけ動かすとする
- P を昇順に並び替えることができるならその場合の操作の順番を答えよ

# 提出コード
```python

```

# 考察
- 必ず端にくる1とN を考えると、それぞれ、1より左にある数、Nより右にある数は決まってくる
  - 同じ場所は一回しか操作できないので、[2,3,5,1,...]だとすると操作後は[1,2,3,5,...]となる。
  - 1の右側は、1の右隣を除いて、2,3,4..と連続している必要がある。
  - 1を移動させたあとは残っている中で最小な数に対して同じ処理を行えば良い
  - 同じところを見るのは高々一回なのでO(N)

# 実装方針
- 現在のmin の値をもつ。
- 右から走査してmin ならmin+=1 , そうでないならmin+1,min+2..と昇順に並んでいるか確認
- 昇順に並んでいないものが2個続いたら不可能なので終了。
- min に到達したら必要に応じて一個左とswapして同じ操作を行う

# 次回への反省
- 

# 参考
