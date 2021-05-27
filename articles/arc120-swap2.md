---
title: "ARC120 Swap2[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python",]
published: true
---

# URL
https://atcoder.jp/contests/arc120/tasks/arc120_c

# 制約
$$ 2<= N<= 2*10^{5}$$
$$ 0<= A[i],B[i] <=10^{9} $$
入力される値は全て整数

# 問題概要
- 長さNの数列A,Bがあって、下記の操作を繰り返すことでAをBにすることができるかどうか、またその必要な最小回数を求めよ
- 操作：
  - A[i]とA[i+1]を入れ替える
  - A[i]に+1
  - A[i+1]に-1

# 提出コード
```python
from collections import defaultdict
n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
a_ = [i+a[i] for i in range(n)]
b_ = [i+b[i] for i in range(n)]
if set(a_) != set(b_):
    print(-1)
    exit()
dict_b = defaultdict(list)
c = [0]*n
for i in range(n):
    dict_b[b_[i]].append(i)
# print(dict_b)
a_count = {}  # a の要素に対して何回出てきたか
for i in range(n):
    if a_[i] in a_count:
        a_count[a_[i]] += 1
    else:
        a_count[a_[i]] = 0
    #print(dict_b[a_[i]], a_count[a_[i]])
    c[i] = dict_b[a_[i]][a_count[a_[i]]]
# c に関して転倒数の計算


def mergecount(A):
    cnt = 0
    n = len(A)
    if n > 1:
        A1 = A[:n >> 1]
        A2 = A[n >> 1:]
        cnt += mergecount(A1)
        cnt += mergecount(A2)
        i1 = 0
        i2 = 0
        for i in range(n):
            if i2 == len(A2):
                A[i] = A1[i1]
                i1 += 1
            elif i1 == len(A1):
                A[i] = A2[i2]
                i2 += 1
            elif A1[i1] <= A2[i2]:
                A[i] = A1[i1]
                i1 += 1
            else:
                A[i] = A2[i2]
                i2 += 1
                cnt += n//2 - i1
    return cnt


print(mergecount(c))

```

# 考察
解説AC
- 自分の考察
  - 操作によって、全体の和は変わらない
  - A[i] = A[i+j] + j となるようなi,j のペアならば移動させることができる
  - 全てのi に対してB[i] = A[i+j] + j となるようなj を重複なく見つけることができれば、AをBに一致させることは可能
  - 普通にやるとあるi に関してO(N)かかるのでどうやって計算量を減らす？？ => 終了
- 解説
  - A_[i] = A[i] + i と変換した配列A_ を作るとA_ とBが同じ数字の集合であれば一致可能
  - 操作にかかる最小の回数は、A_(またはB)が同じ数字を複数もつ可能性があるということを考慮しなければA_ の配列をB の配列でのindex に置き換えると、配列を隣接要素とのswapによって置き換えるという操作になり、これは転倒数の数と一致する
    - 転倒数：https://ja.wikipedia.org/wiki/%E8%BB%A2%E5%80%92_(%E6%95%B0%E5%AD%A6)
    - 例：A_ = [1,4,5,3,2] B = [3,4,5,1,2] の時、B のindex に基づき A_を辞書{1:4,2:5,3:1,4:2,5:3}　のように変換すると、A_をBにする操作は　配列[4,2,3,1,5] を隣接swapによってsortすることと同義になる
  - ここで、A_ が同じ数字を複数もつ時にどういう操作が最小かを考えると、前から貪欲に一番近い同じ数字に変更すれば良いことがわかる。
    - 証明：$$ A_[i] = A_[j] = B[k] = B[l] $$　となるi,j,k,l($$ i<j $$, $$ k<l $$) が存在するとき、iをl にjをkに交換する時のコストはabs(l-i)+abs(k-j) でこれはabs(k-i)+abs(l-j)より大きい

# 実装方針
- A_,B_ の配列を作成
- B_を前から走査する事で、現在出てきた数字をキー、それが何回出てきたかを要素としてもつ辞書dict_bを作る O(N)
  - この時にdefaultdictで辞書にリストを入れると一つのキーに対して複数の値を管理できる
- dict_b を元に、A_をCに変換する O(N)
  - 複数の要素に対応するためにある値に関して現在何回出てきたかをdictで管理する
- Cに関して転倒数を計算するO(NlogN)

# 次回への反省
- A[i] = A[i+j] + j 　から　A_[i] = A[i] + i と変換する操作は頻出なので、自力で気づきたかった
- 転倒数に関してはライブラリも持っていなかったし解けなくてもしょうがないが、名前と概要は聞いたことがあったのでコンテスト中にググれるようになるとよかった
- 転倒数のアルゴリズム理解する

# 参考
公式解説　https://atcoder.jp/contests/arc120/editorial/1921
転倒数のアルゴリズムについて　https://kira000.hatenadiary.jp/entry/2019/02/23/053917
python 辞書にリストを入れる　https://www.haya-programming.com/entry/2018/04/24/002524
転倒数ライブラリはここから借用　https://nagiss.hateblo.jp/entry/2019/07/01/185421