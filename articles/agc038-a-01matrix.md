---
title: "AGC038 A 01Matrix解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","幾何","構築"]
published: true
---

# URL
https://atcoder.jp/contests/agc038/tasks/agc038_a

# 問題概要
- H*Wのgridがある。0,1どちらかの値を持つ
- a,bが与えられ、どの行にも01の少ない方がa,どの列に対しても01少ない方がb個 となるような構築をしろ
  - 不可能な場合はNoを出力

# 制約
$$ 1 <= H,W <= 1000 $$
$$ 0 <=A <= W/2 $$
$$ 0 <= B <= W/2 $$

# 提出コード
```python
h, w, a, b = map(int, input().split())
for i in range(h):
    for j in range(w):
        if i < b:
            if j < a:
                print(1, end="")
            else:
                print(0, end="")
        else:
            if j < a:
                print(0, end="")
            else:
                print(1, end="")
    print()


```

# 考察
- 何度かトライして全然わからず解説ACをしたので、備忘録的に考えたことをのべ公式の解説を述べる
- まず、c,rを0から順番に指定してどのような条件なら構築できるが考えた
  - 片方が0なら他方がどんな値をとっても可能であるが、1のケースでは2以降、h,wの値によらない普遍的な構築を考えることができず諦めた
- 次にシンプルな構築規則で条件をみたす方法がないか考えた
  - 端を固定することでその他の行を同様のh,wの値が小さい部分問題に帰納できるか考えたが難しかった。
- 無理にケースを考えたが、思い付かず。

- 解説
- h*wのグリッドを4分割し、グリッドないは全て同じ文字で埋める
- 左上のグリッドの大きさをa*bにして隣接するグリッドの値を変えると条件を満たす

- 学び
シンプルな構築ケースを考えるとき、ルールベースでシンプルなもの以外に、幾何的にシンプルな構築手法も考える。

# 実装方針

# 参考
- https://img.atcoder.jp/agc038/editorial.pdf
解説pdf




# 参考
