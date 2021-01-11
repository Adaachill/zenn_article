---
title: "ARC111 B. Reversible Cards解説[python]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競プロ","python","連結成分分解","BFS"]
published: true
---

# URL
https://atcoder.jp/contests/arc111/tasks/arc111_b


# 問題概要
- N 枚のカードがあり、両面に正整数の値を持つ
- それぞれのカードに対し、どちらを表にするかを自由に選べる時、表になっている値の種類の最大値を求めよ

# 制約
$$1 <= N<=2*10^{5}$$

# 提出コード
```python
from collections import deque

n = int(input())
e = [[] for _ in range(4 * 10 ** 5 + 10)]  # 隣接リスト
for i in range(n):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    if a != b:
        e[a].append(b)
    e[b].append(a)
seen = [-1] * (4 * 10 ** 5)
# 連結成分に分解して、その中でmin(頂点の個数、辺の個数)を答えにたす
ans = 0
q = deque()
for i in range(4 * 10 ** 5):  # 全頂点：色に対して連結成分を数える
    if seen[i] == -1:
        q.append(i)
        num_e = 0
        num_v = 1
        num_t = 0
        # １つの連結成分に対して
        while q:
            now = q.popleft()
            seen[now] = 0
            for j in e[now]:
                num_e += 1
                if j == now:  # 自己辺
                    num_t += 1
                    continue
                if seen[j] == -1:
                    q.append(j)
                    seen[j] = 0
                    num_v += 1
        # print(e[i], num_e, num_v, i)
        ans += min((num_t + num_e) // 2, num_v)
print(ans)

```

# 考察
- それぞれの値を頂点として、カードをそれをつなぐ辺と考えると
- 入力は自己ループや多重辺を含む無交グラフとして考えられる
 - 連結成分外の値をとることはできないので値の種類数は連結成分に分解して、連結成分に対して独立に考えることができる
 - それぞれの連結成分に対して、取りうる値の種類を考えるとmax は連結成分内の頂点数になる
 - 頂点の数だけ取れないケースがどんなケースかを考えると、連結成分が分岐やサイクルのないpath であるケースのみであることがわかる
 - 証明：適当な頂点を始点として各辺に対して始点に近い頂点の値をとるようにすると、分岐やサイクルがない限り最も遠い頂点以外はとることができる
 - 分岐やサイクルがある場合、その場所で現在の辺の始点から遠い頂点をとることができるので、あとのpathに関しては全頂点をとることができる

# 実装
- 全頂点に対してループをして現在見ていない頂点に対してBFSをすることで連結成分に分解することができる
- 連結成分に関して、辺の数eと頂点数vをカウントすると、ans += min(v,e) で答えがもとまる
- 自己辺が存在するので、自己辺以外は二回カウント、自己辺は一回カウントしてそれを二で割ると正しい辺の数になる