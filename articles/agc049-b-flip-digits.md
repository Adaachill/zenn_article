---
title: "AGC049 B - Flip Digits解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","区間","貪欲"]
published: true
---

# URL
https://atcoder.jp/contests/agc049/tasks/agc049_b

# 問題概要
- 0,1のみからなる長さNの文字列S、Tが与えられる。
- Si = 1 となる任意のi(i>=1) を選び、S[i],S[i-1]のbitを反転させる、という操作を好きな回数行う
- SをTと一致させることは可能か、可能ならそのために必要な最小の操作回数を求めよ

# 制約
$$ 1 <= N <= 5*10-{5} $$

# 提出コード
```python
from collections import deque

n = int(input())
s = input()
t = input()
q = deque()
ans = 0
for i in range(n):
    if s[i] != t[i]:
        if q:  # q が空でない
            # print(i)
            if s[i] == "1":
                ans += i - q.popleft()
            else:
                q.append(i)
        else:
            q.append(i)
if q:
    print(-1)
else:
    print(ans)

```

# 考察
- まず、S[i] = 1　でないと反転できないという制約を除いて考える
  - 操作の性質を考えると、同じ位置で二回反転させることは無意味であることがわかる。
  - 同じ箇所に関して高々1回の操作しか行われないので、S上で反転操作が連続している箇所を１つの区間とする。
  - 操作の終了顔、１つの区間は元の配列から両端の文字のみ反転していることがわかる。（その他の文字列は二回反転されるので元に戻っている）
- 次にS[i] = 1　という制約を入れてかんがえる
  - １つの区間に対して右端以外はS[i]は操作中に二回反転する
  - 反転の順番は区間に対する全操作終了後の文字列に関係ないので区間の右端以外は反転時にS[i]=1になるように操作の順番を選ぶことができる。
  - 区間の右端を1以外にすることは制約的にできないので、区間の右端が1になるように必要に応じて入れ子で両端が不一致となる区間を形成する。


# 実装方針
- Sを左端から順に舐める
  - queue にS[i],T[i]が異なる場所のindexをいれる
  - 現在queueが空でない状態で不一致箇所の S[i]==1ならqueueから１つ取り出して、iとの差分を答えに出す。S[i] == 0　なら区間を形成できないのでqueueにi をいれる

# 次回への反省
- また文字列の比較で　intと比較して if s[i] == 1; 
  バグを産んだ。。。成長がない。
- WAしてから1週間ほど寝かせてACした
  - 不一致の箇所だけに注目して区間で考えれば良いと気づいた後右端が1という条件を貪欲にどう処理すればいいかわからなかった
  - 右端になりうる1がきたら貪欲に処理して良いので、左端になる箇所をqueueに入れるという発想ができて解決

# 参考
https://docs.python.org/ja/3/library/collections.html?highlight=deque%20true
python ドキュメント　deque 
- dequeはpop,append 処理をO(1)でできる。
- 長さが制限されたdequeとかあるんだ。 dequq(f,n) で追加した分だけ捨てられる。
