---
title: "木DPの練習2　AGC009 B - Tournamnt メモ[python]"
emoji: "✨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","数え上げ","木DP"]
published: true
---

ABC187Eで木DPが出て実装でつまづいたので練習する。第二回
第一回(https://zenn.dev/articles/abc_036_d_tree_dp)

# URL
https://atcoder.jp/contests/agc009/tasks/agc009_b

# 問題概要
- N 人からなるトーナメント表があり、１番の人が優勝している
- ２~N番の人に関してどの人に負けたかが与えられる
- トーナメントの深さとして最小なもの（深さとは、根（優勝）からの距離が最も遠いノードまでの距離）

#　制約
N<=10**5

# 提出コード
```python
import sys

sys.setrecursionlimit(10 ** 5 + 100)


def dfs(i):
    child_dp = []
    for j in child[i]:
        child_dp.append(dfs(j))
    child_dp.sort()
    tmp_max = 1  # もし葉ノードだった場合は1を返す
    for j in range(len(child_dp)):
        tmp_max = max(tmp_max, child_dp[-1 - j] + j + 1)
    # print(child_dp, i)
    return tmp_max


n = int(input())
a = [int(input()) - 1 for _ in range(n - 1)]  # 0-indexに直す
child = [[] for _ in range(n)]  # child[i]はiの子集合（i に直接負けた人）
# dp = [-1] * n  # i が負けた時点での深さ：初戦負けで1
for i in range(n - 1):  # aの値はindex+1 すると0-index になっている 2 > index = 0
    child[a[i]].append(i + 1)
print(max(len(child[0]), dfs(0) - 1))

```

# 考察
- 入力の性質：トーナメントは二分木である
  - トーナメント表は連結かつループがないので木である
  - 一度に戦うのは二人なので二分木
  - 今回は節を考えたくないので1を根とした木に変換する
- 言い換え：
  - トーナメント表を1が根で、それ以外のそれぞれのノードが自分が負けた人を親にもつ木として言い換える
- dp[i] : トーナメント表でのノードi が負けた時点での深さ（一回戦負けなら1)
  - 漸化式：dp[i] = max(dp[j]+k) #  j は i の子ノード、kはdp[j]を降順にソートした時の番号、最大なら1
  - 毎回ソートするとO(N*NlogN)かかってTLEする?
    - 計算量としては各ノードが持つ子ノードの数をAとして、sum(A)=N となるような整数列Aに対して計算量が```sum(A[i]logA[i])```と表現できる
    - log の中にまとめると log Pi(A[i]^A[i]) (Piは総乗)となる
    - 任意のi についてA[i]<=Nより、これはNlogNより小さいので全体の計算量はO(NlogN)で抑えられる
  - トーナメントの深さ = max(入力中の1の個数,他のノードが持つ深さ) として表現できる

# 実装
- 木DPの練習としてトポロジカルソートしてから葉からdp を埋めて行こうかと思ったがdfsでの解法を見てしまったこともありdfsで書いた
 - pythonのdefaultの再起上限は少ないのでdfsを書くときは上限を増やす


# 次回解く時のメモ
- 考察に時間がかかった 約50min + 計算量に関して解説を読んだ時間
- ソートするのは思いついたがそれがO(NlogN)で終わるかの証明ができなかった
  - それぞれに対してソートしないといけないからO(N*NlogN)では？と思って沼った
  - 証明としてはlog の中にまとめれば良い
- 実装
  - 辺の持ち方とスコアの持ち方に手間取った
    - 隣接行列のみでとけた
  - DFSで再帰的に計算することでdp の形で値を保持せず簡潔にかける
  - ```max(len(child[0]), dfs(0) - 1)``` と書いたが```len(child[0]) < dfs(0) - 1``` はdfs内のj=len(child[0])-1をとる時にchild_dp[-1 - j] + len(child[0]) になるので不要だった
# 参考にしたURL
- http://vartkw.hatenablog.com/entry/2017/03/13/215908
- https://atcoder.jp/contests/agc009/tasks/agc009_b/editorial
