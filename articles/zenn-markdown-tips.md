---
title: "zennでの記事執筆時のtips[備忘録]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["zenn","備忘録","latex"]
published: true
---

# 執筆時の環境的なこと
- 基本的にはローカルのエディタを使って執筆する

@[card](https://zenn.dev/zenn/articles/editor-guide)
- 記事のpreviewをする際にはターミナルで `npx zenn preview` を実行してブラウザ上で確認する

# 数式の記述
- 数式は`$$` で囲ってLaTeX形式で記述する
- 基本的にはLaTeX形式でいいが、KaTeX を使用しているので不安な時やうまく表示されない時はKaTeX の公式サイトを参考に数式を書く(一部LaTeX では使える記法が使えなかったりする 例：`aCb` みたいな書き方は多分できない)

https://katex.org/docs/supported.html

- 文中に埋め込みたい時は`$`一個で数式を挟むが、その時に$と数式間に空白を入れるとうまく表示できないので空白は入れない

# その他
- 注釈は`[^1]`で文中に入れ込み、文末に`[^1]:内容`の形式で記述するか、インラインで`[脚注の内容その2]`のように書くこともできる
- 区切り線 - 5つで区切り線になる`-----`

参考：
https://zenn.dev/zenn/articles/markdown-guide