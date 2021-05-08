---
title: "ABC199 IPFL解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","クエリ問題"]
published: true
---

# URL
https://atcoder.jp/contests/abc199/tasks/abc199_c

# 制約
$$ 1<= N<= 2*10^{5}$$
$$ 1<= Q<= 3*10^{5}$$

# 問題概要
- 長さ2Nの文字列Sがある。クエリが全部処理した後のSを答えよ
- T,A,B の3つの整数からなるQ個のクエリが与えられる
- T = 1 の時、SのA文字目とB文字目を入れ替える
- T = 2　の時、Sの前半N文字目と後半N文字目を入れ替える

# 提出コード
```python
def solve(N, S, Q, T, A, B):
    ans = []
    for i in S:
        ans.append(i)
    swap_flag = False
    for i in range(Q):
        if T[i] == 1:
            if swap_flag == False:
                idx1, idx2 = A[i], B[i]
            else:
                if A[i] <= N:
                    idx1 = A[i] + N
                else:
                    idx1 = A[i] - N
                if B[i] <= N:
                    idx2 = B[i] + N
                else:
                    idx2 = B[i] - N
            ans[idx1-1], ans[idx2-1] = ans[idx2-1], ans[idx1-1]
        else:
            swap_flag = not swap_flag
        #print("".join(ans), swap_flag)
    if swap_flag:
        ans = ans[N:] + ans[:N]
    return "".join(ans)


if __name__ == "__main__":
    N = int(input())
    S = input()
    Q = int(input())
    T, A, B = [0]*Q, [0]*Q, [0]*Q
    for i in range(Q):
        T[i], A[i], B[i] = map(int, input().split())
    print(solve(N, S, Q, T, A, B))

```

# 考察
- 制約から毎回の操作を愚直に行うとO(NQ)で通らない
  - T = 1 の操作はO(1)でできるので、T = 2 の時の操作を効率化したい
- 文字列Sの長さ、構成する文字は変わらずその順番だけ変わるので最初の文字列の各文字に対して、今それぞれが何番目にいるか、という写像を持てばいい
- ここからの展開がわからず解説を読んだ

# 解説
- T = 2 では前後半の文字列が入れ替わるので、入れ替わっているかどうかをflagで情報として保持する
- T = 1 での操作は入れ替わってなければそのまま入れ替える、入れ替わっている場合は現在の文字列でのindex iと実際の文字列のindex jは1-indexで
   i <= N　なら j = i + N
   i > N なら j = i - N
  となる。
- 最後のクエリまでswap操作を行った段階でflagに応じて 前後半の文字列が入れ替えたものを出力すればOK

# 実装方針
- flag をもつ
- 文字列だと直接操作できないのでリストに１文字ずつ入れる
- 最初は1-index で添字の変化を考えて、0-index にしてswapする

# 次回への反省
- 久しぶりにコンテスト以外で問題を解いてみたが茶dif下位を解けず悲しい。
- ランダムチェッカーが作りやすいように関数化して描いてみたが今後も続けてみる

# 参考
