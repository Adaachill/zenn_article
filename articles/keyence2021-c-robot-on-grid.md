---
title: "Keyence2021 C - Robot on Grid解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python",]
published: true
---

# URL
https://atcoder.jp/contests/keyence2021/tasks/keyence2021_c

# 制約
$$ 2 <= H,W<=5000 $$
$$ 0<= K <= min(HW,2*10^5)$$
$$ 1 <= hi<= H  $$
$$ 1 <= wi<= W $$

# 問題概要
- H*Wのgrid がある。それぞれのマスにはR,D,Xのいずれかが書き込める
- Kこのマス(hi,wi)を選んで文字Ciを書き込んだ
- 残りのH*W-K のそれぞれの書き込み方（3^(H*W-K)通り)での移動方法の総和を998244353で割ったあまりを答えよ
- (1,1)から(H,W)まで移動できるようなルート　
- R ならj+1 , Dならi+1に移動。X なら両方いける
# 提出コード
```python
def xgcd(a, b):
    x0, y0, x1, y1 = 1, 0, 0, 1
    while b != 0:
        q, a, b = a // b, b, a % b
        x0, x1 = x1, x0 - q * x1
        y0, y1 = y1, y0 - q * y1
    return a, x0, y0

def modinv(a, m):
    g, x, y = xgcd(a, m)
    if g != 1:
        raise Exception('modular inverse does not exist')
    else:
        return x % m

h, w, k = map(int, input().split())
dp = [[0] * (w + 2) for _ in range(h + 2)]
grid = [[0] * (w + 2) for _ in range(h + 2)]
for i in range(k):
    p, q, r = map(str, input().split())
    p = int(p)
    q = int(q)
    if r == 'X': grid[p][q] = 1
    if r == 'R': grid[p][q] = 2
    if r == 'D': grid[p][q] = 3
mod = 998244353
inv3 = modinv(3,mod)

dp[1][1] = 1
for i in range(1, h + 1):
    for j in range(1, w + 1):
        if grid[i][j] == 1:
            dp[i + 1][j] += dp[i][j]
            dp[i][j + 1] += dp[i][j]
        elif grid[i][j] == 2:
            dp[i][j + 1] += dp[i][j]
        elif grid[i][j] == 3:
            dp[i + 1][j] += dp[i][j]
        else:  # 空白文字
            dp[i + 1][j] += 2 * inv3 * dp[i][j]
            dp[i][j + 1] += 2 * inv3 * dp[i][j]
        dp[i + 1][j] %= mod
        dp[i][j + 1] %= mod
# print(dp)
print(int(dp[h][w] * pow(3, h * w - k, mod)) % mod)
```

# 考察
- 問題から全探索はできないので、同じような状態をまとめる必要がある。
- それぞれのパスに対して、それを使ってゴールする書き込み方が何通りあるかを数えたい
  - 書き込み方はそのパス上にある書き込まれていないマスをi個とした時、
  $$2^{i}*3^{HW-i-K}$$となる
  - 証明：パス上のマスはXかRorDのうちパスに従うものの２通りあり、パス上でないマスはどの文字でも良いため
- 陽にパスに対して全探索すると1000C500通りでTLEするのでDPでまとめる必要がある
  - dp[i][j] := (i,j)に到達するような書き込み方でのパスの総和　としたい
  - 一方で、それぞれのpathに対して何個の書き込まれていないマスを通ったかの情報を持ちたい
  - しかし、それを状態としてもつとdp[i][j][k] となって、オーダーが$$5000*5000*5000$$でTLEする。
    - 書き込まれていないマスを通ったらパスの総和を3で割ることで帳尻を合わせる
  - 答えはdp[h][w]*　3^(HW-K) となる。
- 漸化式を考えると、配るDPの方が元のマスの値を計算しやすいので、それで考えて
  - if grid[i][j] == "X" : dp[i+1][j] += dp[i][j],dp[i][j+1] += dp[i][j]
  - elif grid[i][j] == "R": dp[i][j+1] += dp[i][j]
  - elif grid[i][j] == "D" : dp[i+1][j] += dp[i][j]
  - else: p[i+1][j] += 2/3*dp[i][j],dp[i][j+1] += 2/3*dp[i][j]
  という式になる

# 実装方針
- 3で割り続けるので、初期値をどうするかがきも
  - modinv 逆元を使う。

# 次回への反省
- 結局が逆元がわからなくて友人に聞いて解いたので解説AC
- https://atcoder.jp/contests/keyence2021/submissions/19492572　
Pyton3.8以降だとpow(a,-1,mod)で逆元を計算することができるが、それだと速度的に間に合わないので
- https://atcoder.jp/contests/keyence2021/submissions/19492601
自分で逆元を計算してPypyで提出する
- 今回のような(5000*5000とか）大きい範囲の文字列のgridを持つときは、数字に置き換えた方が良い。
- 番兵をおくなら1-index そのまま受け取ると良い

# 参考
https://tex2e.github.io/blog/crypto/modular-mul-inverse　
python3でのモジュラ逆数の求め方