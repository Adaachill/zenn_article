---
title: "ABC198 D Send More Money[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","全探索","premutation"]
published: True
---

# URL
https://atcoder.jp/contests/abc198/tasks/abc198_d

# 制約
$$1 <= max(len(S1),len(S2),len(S3)) <= 10$$

# 問題概要
- 英子文字からなる文字列S1,S2,S3が与えられる
- 覆面算　S1+S2 = S3 を説くような対応する正の整数の組みN1,N2,N3 を求めよ（存在するならば条件を満たす組みを1つ出力、しないならそれを報告）


# 提出コード
```python
import itertools
s = [input() for _ in range(3)]
alphabet_hist = [0]*26
for i in s:
    for c in i:
        alphabet_hist[ord(c)-ord("a")] += 1
# 後のpermutation_lis の割り当てのために、出現回数が0のものを除いた上でそれぞれの文字列が何番目に現れるかを記録する
num_alp = len([i for i in alphabet_hist if i > 0])
idx_alp = [0]*26  # 出現回数が1以上の文字に対して前から何番目かをもつ

cur_idx = 0
for i in range(26):
    if alphabet_hist[i] != 0:
        idx_alp[i] = cur_idx
        cur_idx += 1

if num_alp > 10:
    print("UNSOLVABLE")
else:
    num_list = [i for i in range(10)]  # 0~9までの数字のリストを並び替える
    permutation_lis = itertools.permutations(num_list)
    permutation_lis = permutation_lis  # 効率化のため使う文字数までに縮める
    for case in permutation_lis:
        # print(case)
        # それぞれのケースに対してS1,S2,S3に数字を当てはめて和が一致するか？
        # 先頭に0がつくケースは除外する
        flag_first_zero = False
        num_s = [0]*3
        for i in range(3):
            tmp = 0
            for c in s[i]:  # 最上位位から順番につける
                # 次に数字がくる場合は10倍して次の数字をたす
                tmp = tmp*10 + case[idx_alp[ord(c)-ord("a")]]
                if tmp == 0:
                    flag_first_zero = True
            if flag_first_zero:
                break
            num_s[i] = tmp
        if flag_first_zero:
            continue
        if num_s[0] + num_s[1] == num_s[2]:
            for i in num_s:
                print(i)
            exit()
    print("UNSOLVABLE")

```

# 考察
- 通常の覆面算を解く場合を考える
  - S1 or S2とS3の同じ桁で同じ文字があるかをみて同じものがあれば、残りは0 or 繰り上がりがあって9になるので、絞れる
  - 最上位の桁を見て可能性のある数字を絞る（S1,S2の桁がS3と同じ桁まであるならそれより小さくなる）
  - => 実装に落とすのは難しそう。全探索の中に組み込むくらいかな
- 全探索
 - 英子文字は26文字まで、数字は0~9の10種類、かつ違う文字なら違う数字が対応するので英子文字が11種類以上出てきたらその時点で不可能
 - 文字種が10以内なら0~9の割り当て方を全パターン試すとmaxで10!で4*10^7 程度のパターンになる。
 - 割り当てを決めた時にそれで覆面算が満たすかどうかの判定はO(1)でできる（文字列の作成にO(len(S))かかる）

# 実装方針
1. 入力（文字列で受け取り）
2. アルファベットに対してヒストグラムを作って文字種数を数える
3. 全ての割り当てをitertools.permutation で作成する
4. それぞれの割り当てに対して、覆面算を満たすかを計算する


# 次回への反省
- continue を使うべきところでbreak を使っていてバグらせた。（ありがち）
- for case in permutation_lis　の所で使わない文字列は飛ばすような処理を入れるとよかった（最悪計算量は変わらないが）
  - 3種類の文字列しか使わないなら、permutation_lis の4文字目以降は違っていても覆面算には影響しないので
  - itertools objectのpermutation_lis を使うと無理？DFSとかで既に使った文字を使用しないように埋め込むと書けそう
# 参考
