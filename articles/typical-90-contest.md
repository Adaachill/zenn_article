---
title: "典型90問緑コーダーの完走記録"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","典型"]
published: false
---

# URL

# 制約
$$ <= <= $$
$$ <= <= $$

# 問題概要
- 

# 提出コード
```python

```

# 考察
- 

# 実装方針

# 次回への反省
- 

# 参考

# 全体の感想
- 5/24 から一日一問は考えようと思う(この時4問しか解いていない)
- 

# 045_難易度6
## 概要
- ユークリッド平面上のNこの点をKこのグループに分ける
- この時、同一グループ内での二点間距離の最大値が最小になるようなグループの分け方を考え、その時の最小値を答えよ
## 考察
- K,N <= 15 なので、計算量はそれほど考えなくて良い
- 先に全点の二点間の距離を計算することができるO(N^2)
- 階層的クラスタリングみたいな感じで最初全点を別のグループとして、距離が近くものから順番に統合していくと良さそう
## アルゴリズム
- 全点の二点間の距離を計算してソートする
- 最初全点を別のグループとして、距離が近くものから順番に統合していく
  - 小さいものからとって、その二点が含むグループを統合する
  - グループを統合したらそのグループ内の二点の距離を距離リストから全部削除する
- 上の操作をN-K回繰り返す
データ構造としては、ソート関係を維持して更新できるheap を使う
消去したやつがどの点を同じグループに属するかはどうやって管理する？親indexをリストでもつ 

# 046_難易度3
## 概要
- 長さNの数列A,B,Cがある。
- A[i]+B[j]+C[k]が46の倍数になるような(i,j,k)のくみが何個あるか？
## テーマ
- mod でまとめる
- ヒストグラムを使う
## 考察
- 最初半分前列挙してsortするのかと思ったが、46のmod だけ見ればいいので、46のあまりでそれぞれヒストグラムを作って、46のあまりでループを回せばヒストグラム作成にO(N),ペアの数の数え上げに46*46で計算できる
## 実装
```python
n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
c = list(map(int, input().split()))
a_hist, b_hist, c_hist = [0]*46, [0]*46, [0]*46
for i in range(n):
    a_hist[a[i] % 46] += 1
    b_hist[b[i] % 46] += 1
    c_hist[c[i] % 46] += 1
ans = 0

for i in range(46):
    for j in range(46):
        tmp: int = a_hist[i]
        tmp *= b_hist[j]
        tmp *= c_hist[(-i-j) % 46]
        ans += tmp
print(ans)
```

# *047_難易度7
## 概要
- R,G,B の3文字からなる長さNの文字列S,Tが与えられる。
- N*Nのgridがあり、次の規則で色を塗る
- S[i],T[j]が同じ文字ならその色、違う色ならどちらでもない色に塗る
- 右下がりの斜めの列で、一色のみからなる列が何本あるか？
## テーマ

## 考察
- naive に考えると、全てのマスに対して色を割り当てるのに、O(N^2),斜めの列（2*N-1)本の色の判定にO(N^2)かかるので、小課題１は解ける
- O(N^2)から計算量を落とすことを考えると、２つのstep両方に関して計算量を落とすか、そもそも計算フローを変える必要がある。
- 3色しかないことを利用して計算量を落としたい
- 典型要素がわからない。。。

## 反省
小課題1のコードを書くのにひたすら間違えた。
x,y の座標を逆にしたり、タイポしたりしていた。。。
これで40minくらいかけているのはだめ。。

## 実装
```python
n = int(input())
s = input()
t = input()
# めんどくさいので一気に斜めの列をみて、それに対してその色を決めていく
color = [""]*(2*n-1)  # ""なら来た色を埋める
ans = 0
for x_y in range(-n+1, n):
    # -n+1 <= x-y <= n-1まで探索する
    ng_flag = False
    for x in range(n):
        y = x - x_y
        if y < 0 or y >= n:
            continue
        else:
            if color[x_y + n-1] == "":
                if t[x] == s[y]:
                    color[x_y + n-1] = t[x]
                else:
                    for i in ["R", "G", "B"]:
                        if t[x] != i and s[y] != i:
                            color[x_y + n-1] = i
            else:
                if t[x] == s[y]:
                    if color[x_y + n-1] != t[x]:
                        ng_flag = True
                else:
                    for i in ["R", "G", "B"]:
                        if t[x] != i and s[y] != i:
                            if color[x_y + n-1] != i:
                                ng_flag = True
    if ng_flag == False:
        #print(x_y)
        ans += 1

print(ans)

```

# 048_難易度3
## 概要
- Nこの問題があり、それぞれの問題につき1min でBi,2min でAi 点を獲得できる
- K分で最大何点取れるか？
## テーマ
sort
## 考察
- 単純にBiとAi-Bi をまとめてsortしたものを上からK個の和をとれば良い
- ソートするのに、O(NlogN) 和をとるのにO(K)なので、O(NlogN)で計算できる
## コード
```python
n, k = map(int, input().split())
a, b = [], []
for i in range(n):
    c, d = map(int, input().split())
    a.append(c)
    b.append(d)
ab = []
for i in range(n):
    ab.append(b[i])
    ab.append(a[i]-b[i])
ab.sort()
print(sum(ab[-k:]))
```


