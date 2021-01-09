---
title: "ABC115 D - Christmas解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","漸化式","再帰"]
published: true
---

# URL
https://atcoder.jp/contests/abc115/tasks/abc115_d

# 問題概要
- 次の規則で成り立つ文字列がある。
- S[0] = P,S[i+1] = B+S[i]+P+S[i]+B
- S[N-1] の先頭からX文字目までにPは何文字あるか？

# 制約
$$ N<=50 $$

# 提出コード
```python
#!/usr/bin/env python3
n, x = map(int, input().split())
len_n = [0] * 51
p_n = [0] * 51
len_n[0] = 1
p_n[0] = 1
for i in range(1, 51):
    len_n[i] = 2 * len_n[i - 1] + 3
    p_n[i] = 2 * p_n[i - 1] + 1


def dfs(n, x):
    if x == 0:
        return 0
    if n == 0:
        return 1
    if x <= len_n[n - 1] + 1:
        return dfs(n - 1, x - 1)
    elif x <= 2 * len_n[n - 1] + 2:
        return 1 + p_n[n - 1] + dfs(n - 1, x - 2 - len_n[n - 1])
    else:
        return p_n[n]


print(dfs(n, x))
```

# 考察
- Naive にS[N-1]までS[i]を構成することができるかを考える
  - S[i]のB,Pの文字数をそれぞれfb[i],fp[i]とすると

$$ fb[i+1] = 2*fb[i]+2,fp[i+1]=2*fp[i]+1 $$ 
  
  となる
  - 2^50 は10^15 オーダー となるのでこれを文字列として持つのは無理。
- 再帰的に分割していく必要がある

# 実装方針
- 再起ということで現在のSのindex と文字数を引数にもつdfsを用いる
  - dfs(n-1,x)を求めたい
  - dfs(i,x) は下の方のi-1バーガーを完全に含まないならdfs(i-1,x-1)と表せる。完全に含むなら真ん中のPも加えて1 + p_n[n - 1] + dfs(n - 1, x - 2 - len_n[n - 1])　となる


# 次回への反省
- 再起ならdfs という枠組みに落とした方が楽
- dfs の引数としてどの状態を持つ必要があるかを考えることが肝

# 参考
