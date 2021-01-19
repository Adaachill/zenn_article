---
title: "AGC034 B - ABC解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","python","端から操作","発想"]
published: true
---

# URL
https://atcoder.jp/contests/agc034/tasks/agc034_b

# 制約
$$ 1<= |s| <= 2*10^{5}$$
s の各文字はA,B,Cのどれか

# 問題概要
- s の連続した部分列でABCであるものを１つ選びBCAに書き換えるという操作をできるだけ多く行えるようにする時最大の操作回数を求めよ

# 提出コード
```python
s = input()
l_s = len(s)
ans = 0
consequence_A = 0
i = 0
while i < l_s:
    if s[i] == "A":
        consequence_A += 1
    elif i < l_s - 1 and s[i : i + 2] == "BC":
        ans += consequence_A
        i += 1
        # print(ans, i, consequence_A)
    else:
        consequence_A = 0
    i += 1
print(ans)


```

# 考察
- サンプルを見ながらどういうケースなら操作できるかを考えると、
- ABCが連続している or ABCの右端にAが左端にBCが連続している時操作できることがわかる
- この操作を言い換えると、AをBCが右隣にある時にBCの左に移動させる操作と言い換えることができて、Aは右がBCorA以外の場合は移動させることができない。
- 以上の条件を元に左端からAを発見したら何個ずらせるか、計算すると答えがもとまる

# 実装方針
- 今Aが何個連なっているかを変数として持つ
- 次にきた文字列がBC or A 以外なら0に戻す
- BCを超える度にAが何個連なっているか　をans にたす

# 次回への反省
```python
for i in range(l_x):
  if ~~
  elif i < l_s - 1 and s[i : i + 2] == "BC":
        ans += consequence_A
        i += 1
  else: ~~
```
と書いてバグを産んだ。。

C++と同じ感覚でfor ループの中でイテレーターを+1しても意味がない
pythonのfor ループは in ~~ で書いたシークエンス型を順番にループするという動作なので、ループの中で イテレータを変更しても次のループには影響がない。（参考)

  

# 参考
https://docs.python.org/ja/3/tutorial/controlflow.html?highlight=for
>Python の for 文は、読者が C 言語や Pascal 言語で使いなれているかもしれない for 文とは少し違います。 (Pascal のように) 常に算術型の数列にわたる反復を行ったり、 (C のように) 繰返しステップと停止条件を両方ともユーザが定義できるようにするのとは違い、Python の for 文は、任意のシーケンス型 (リストまたは文字列) にわたって反復を行います。反復の順番はシーケンス中に要素が現れる順番です。

https://ami-atcoder.hatenablog.com/entry/20190605/1559712377
BCをあらかじめD とかに置換するともっと簡単にかける。（確かに）