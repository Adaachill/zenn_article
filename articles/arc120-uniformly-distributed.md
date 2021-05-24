---
title: "ARC120 B Uniformly Distributed[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python",]
published: true
---

# URL
https://atcoder.jp/contests/arc120/tasks/arc120_b

# 制約
$$ 2<= H,W <=500 $$
S はR,B,　. からなる長さWの文字列

# 問題概要
- R,B,　. からなるH*W のgridが与えられる。
- . のマスはR,Bのいずれかに塗ることができる
- 以下の条件を満たすような塗り方の総数を998244353で割ったものを求めよ
- 条件：マス(1,1)から(H,W)に最短距離で移動するようなパスのうち、全てのパスに対してそのパス中に存在するRの数が同じ

# 提出コード
```python

mod = 998244353
# (0,0) からのマンハッタン距離がi の全てのgridを調べて
# R,B が両方あるなら0 確定、片方あって.があるなら*1 、.だけなら*2する
h, w = map(int, input().split())
grid = []
for i in range(h):
    grid.append(input())
ans = 1
for i in range(1, h+w-1):
    r_flag = False
    b_flag = False
    for x in range(w):
        #print(i, ans)
        if i-x < 0 or i-x > h-1:
            continue
        if grid[i-x][x] == "R":
            r_flag = True
        elif grid[i-x][x] == "B":
            b_flag = True
    if r_flag and b_flag:
        print(0)
        exit()
    elif r_flag == False and b_flag == False:
        ans *= 2
    ans %= mod
    #print(r_flag, b_flag)
if grid[0][0] == ".":
    ans *= 2

print(ans % mod)
```

# 考察
- 最初の印象としてDPで状態をまとめられそう
  - 内部の適当な点(i,j)を考えると、(1,1)=>(i,j)のパス、(i,j)=>(H,W)のパスは全てRの数が同じ というように分割できる
  - 長さ1のパスまで分割できるのでは？
  - (1,1)からマンハッタン距離で1ずつ伸ばしていくとマンハッタン距離がi の全てのマスはすべてRの数が同じになる
    - つまり全てR or 全てB
- マンハッタン距離がi の全てのマス を0<=i<=H+W-2 まで全て走査するのはO(H*W)ででき、その際に距離が同じマスにすべてに対してR,B 両方に揃えられる、片方に揃えられる、揃えられないのいずれかに分類するとO(H*W)にできる。

# 実装方針
- マンハッタン距離がi のマスを走査して、その中にR,B　のマスがあるかをflagで管理する
- 両方flag が立っているなら揃えられないので、ans = 0
- 片方flag が立っているならその色に揃えられるので ans *= 1
- 両方flag が立っていないなら両方に揃えられるので ans *= 2

# 次回への反省
- mod をとるのを忘れていてWA出した。
- 忘れないように最初にmod を書いていたけど末尾に書いた方が良いかも。 print(ans%mod) とか書いておけば途中でmodとる必要がある時も忘れなそう。

# 参考

