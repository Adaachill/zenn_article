---
title: "ABC202 D aab aba baa[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","文字列の辞書順","combination"]
published: true
---

# URL
https://atcoder.jp/contests/abc202/tasks/abc202_d

# 制約
$$　1 <= A,B <= 30　$$
K はありうる総数以下

# 問題概要
- A個のa とB個のb からなる長さA＋Bの文字列のうち、辞書順でK番目のものを求めよ

# 提出コード
```python

```

# 考察
- 辞書順で何番目か？はよくある問題。桁dp とかで解く印象
- 最初の文字から順番に決めることができる
- 現在 i 文字目まで決まっているとして、残りのA+B-i 文字の中でa がx 、bがy 残っているとすると、次にa がくるのは、
$$ k <= {}_x+y-1 C_x-1 $$ 
であるときでbを使う場合は
$${}_x+y-1 C_x-1 $$ 
をkから引いて行けば良い

# 実装方針
1. 先頭の文字から順番でa or b を決める(for)
2. if $$ k <= {}_x+y-1 C_x-1 $$  なら　a にしてそのまま、それ以外ならその数を引いてb にする　を 最後の文字まで続ける

# 次回への反省
- 最初の実装では片方の文字がなくなっても実装方針で書いたループを継続していたので存在しない文字列（全部a とか）を生成していた（以下最初に書いたWAコード）
```python
a, b, k = map(int, input().split())
ans = ""


def comb(a, b):
    """return combination aCb

    Args:   
        a ([int]]): 
        b ([type]): 
    Computation Complexity:
        O(b)
    """
    numerator = 1
    denomirator = 1
    for i in range(b):
        numerator *= a-i
        denomirator *= b-i
    return numerator//denomirator


exist_a = a
exist_b = b
for i in range(a+b):
    if i == a+b-1:  # 最後だけは例外処理
        if exist_a:
            ans += "a"
        else:
            ans += "b"
        break
    if k <= comb(exist_a+exist_b-1, exist_a-1):
        ans += "a"
        exist_a -= 1
    else:
        k -= comb(exist_a+exist_b-1, exist_a-1)
        ans += "b"
        exist_b -= 1
print(ans)
```
- 小さいテストケースでどうなるかサンプルとは別に試す
  - ある値が0になるときや負になるようなケースは特に注意してそのままでいいか検討する

# 参考
