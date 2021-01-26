---
title: "CODE FESTIVAL2017 qualA C - Palindromic Matrix解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","実装","回文"]
published: true
---

# URL
https://atcoder.jp/contests/code-festival-2017-quala/tasks/code_festival_2017_quala_c

# 制約
$$ 1<= H,W  <= 100 $$


# 問題概要
- H*Wの行列Aがあり、それぞれのマスには小文字が与えられている
- Aの要素を自由に並び替えてAのどの行、列もそれぞれ回文になっているようにできるか判定せよ

# 提出コード
```python
h, w = map(int, input().split())
a = [input() for _ in range(h)]
hist = [0] * 26
for i in range(h):
    for j in range(w):
        hist[ord(a[i][j]) - 97] += 1
ng = 0
if h % 2 == 0 and w % 2 == 0:
    for i in range(26):
        if hist[i] % 4 != 0:
            ng = 1
elif h % 2 and w % 2:
    # 両方奇数の時は、回数奇数個のものが1つ、かつ回数がmod4で2のものの個数が(h+w-2)//2と同じ、かつ他が4で割り切れればOK
    odd = 0
    mod_two = 0
    for i in range(26):
        if hist[i] % 4 == 2:
            mod_two += 1
        elif hist[i] % 2:
            odd += 1
    if (
        odd == 1
        and ((h + w - 2) // 2) % 2 == mod_two % 2
        and ((h + w - 2) // 2) >= mod_two
    ):
        ng = 0
    else:
        ng = 1
    # print(2, ng)
else:
    if w % 2:
        h, w = w, h  # hが奇数、wが偶数にする
    mod_two = 0
    for i in range(26):
        if hist[i] % 4 == 2:
            mod_two += 1
        elif hist[i] % 2:
            ng = 1
    if mod_two % 2 != (w // 2) % 2 or mod_two > (w // 2):
        ng = 1
    # print(3, ng)
if ng:
    print("No")
else:
    print("Yes")
```

# 考察
- 自由に並び替えることができるので各文字のヒストグラムをもてば良い
- それぞれの行、列で出てくるのは左右対称になる(十分条件)
- H,Wが偶数なら全て2つ以上存在する、奇数なら真ん中は1つは1つのみでも良い
  - 両方奇数:奇数個は1つのみ、h+w-1は個数がmod4で2のものが存在できる。 他は4つ組で構成される
  - h奇数w偶数:w/2組は2個存在できる。他は4つ組で構成される
  - 両方偶数：全て4つ組で構成できる。

# 実装方針
- まず26文字それぞれの回数のヒストグラムを作る
- 両方偶数なら全てが4で割り切れるかを判定
- 片方奇数ならmod4で2のアルファベットの個数がw/2と同じならOK
- 両方奇数の時は、回数奇数個のものが1つ、かつ回数がmod4で2のものの個数が(h+w-2)//2と同じ、かつ他が4で割り切れればOK
- 考察漏れ：例えばmod4で2の個数が　多すぎると偶奇が一致していてもだめなので、そこを実装する
  - 例えばh=1,w=6でaabbcc という文字列の時は

# 次回への反省
- 論理式の記述を間違っていて、or にすべきをandにして、20min くらいdebugしていた。
- 実装自体にも1H近くかかり反省。。
- 全体で数える必要があるのがmod_twoとodd しかないので先にかくともっとスッキリかけた
# 参考
