---
title: "ABC177 E - Coprime 解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","最大公約数"]
published: false
---

# URL
https://atcoder.jp/contests/abc177/tasks/abc177_e

# 制約
$$ 2<=N <=10^{6} $$
$$ 1<=A[i] <=10^{6} $$

# 問題概要
- 長さNの整数列Aがある。
- この時数列中の任意の２要素のGCDが1である時、Aはpairwise coprime であるという
- また、Aがpairwise coprimeではなく、GCD(A1,A2,,An)=1の時、Aはsetwise coprimeであるという
- Aが与えられた時、pairwise coprime,setwise coprime,そのどちらでもないかを判定して出力せよ

# 提出コード
```python

```

# 考察
- setwise coprimeの判定はGCD((GCD(GCD(A1,A2),A3),A4),,,) と端から順番にGCDを計算して今までのGCDの結果とさらにGCDを計算していくと計算量O (NlogA)で抑えられるので間に合う
-  pairwise に関しては、Aの要素の全ての素因数を全列挙して重複するものがないかを調べると良い（これでも間に合いそうだけど2から順番に素数で割っていくと良さそう）

# 実装方針

# 次回への反省
- 

# 参考
