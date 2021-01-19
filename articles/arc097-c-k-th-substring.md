---
title: "ARC097 C - K-th Substring解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","python","部分文字列","文字列の順序","愚直実装"]
published: true
---

# URL
https://atcoder.jp/contests/arc097/tasks/arc097_a
# 制約
$$ 1<= |S|<=5000 $$
$$ 1<= K<= 5$$
s は英子文字からなり、異なるsubstringをK個以上持つ

# 問題概要
- 文字列sの異なるsubstringのうち、K 番目に小さいものを出力せよ
# 提出コード
```python
#!/usr/bin/env python3
# input
s = input()
l = len(s)
k = int(input())
alphabet = [[] for _ in range(26)]
for i in range(l):
    alphabet[ord(s[i]) - 97].append(s[i : min(l, i + 5)])
# print(alphabet)
while k > 0:
    for i in range(26):
        if len(alphabet[i]) == 0:
            continue
        alphabet[i] = list(set(alphabet[i]))
        alphabet[i].sort()
        used = []
        # print(alphabet[i])
        for j in alphabet[i]:
            # print(j)
            for m in range(1, len(j) + 1):
                if j[:m] not in used:
                    used.append(j[:m])
                    k -= 1
                    # print(used, k)
                    if k == 0:
                        print(used[-1])
                        exit()


```

# 考察
- 愚直に考えると、部分文字列  |s|+1 C 2   個を全て比較してO(|s|^2log|s|)で解けるがTLE
- O(|s|^2)までは回るので、結構愚直な書き方でOK
- K が小さいことから、最初の文字だけで区別して良さそう
- 出てきた文字の中で最も小さいものから辞書順で長さK以下の部分文字列を全てとってきてsortする

# 実装方針
- 辞書順に小さい文字からそこを最初の文字にした長さK以下の部分文字列を取り出す。
- sortして小さいものから長さをインクリメントしながら１つずつ取り出す
- 次の文字で同様の操作を行う

# 次回への反省
- 愚直な実装に苦手意識がある
- 短期記憶がくそなので、筋肉実装の時、イメージが湧かず、適当に書いてしまう
  - できるだけ全探索などの複雑な分岐を書かなくても良いように設計する。
- python setだとソートされる訳ではない。（入り方は下位bitでのhash値？）
- 計算量を考えると、長さ5以下の部分文字列を全てリストに入れてソートしても解ける。リストに入るのは |s|*5 程度なので、|s|をNとしてO(NlogN) となる。(コメントを受けて追記：ありがとうございます。)

```python
s = input()
l = len(s)
k = int(input())
substring_all = []
for i in range(l):
    for j in range(5):
        substring_all.append(s[i : i + j + 1])
substring_all.sort()
pre = ""
idx = 0
while k > 0:
    if substring_all[idx] != pre:
        k -= 1
        pre = substring_all[idx]
    idx += 1
    if k == 0:
        print(pre)
        exit()

```

# 参考
https://teratail.com/questions/271942
python set 順序
https://docs.python.org/ja/3/library/stdtypes.html#set-types-set-frozenset
python 公式ドキュメント
>コレクションには順序がないので、集合は挿入の順序や要素の位置を記録しません。