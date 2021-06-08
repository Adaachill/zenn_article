---
title: "vscode snippetsのフォルダの場所[備忘録]"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vscode","備忘録","snippets","生産性向上"]
published: true
---

# 全体的なsnippetsの場所
- `Code/User/snippets/`ディレクトリ以下に各言語ごとのsnippetを記述したjsonファイルをおく
 - Mac なら`~/Library/Application\ Support/Code/User/snippets/言語.json` にこの形で各言語ごとのsnippetsがあるのでそこから編集する
 - Shortcut(Macならcommand + Shift + P) から `Preference:Configure User Snippets` から対象の言語を選んでjsonファイルを開くこともできる

# プロジェクトごとに反映されるには
- プロジェクト内の`.vscode` ディレクトリに作って反映させることができる

参考：
@[card](https://qiita.com/282Haniwa/items/82828c6a566e3e7e047d)

# 余談：snippet generator
@[card](https://snippet-generator.app/)
が便利。各種のエディタに合わせた形式のsnippetを簡単に作成してくれるので、これで作成して`.json`に貼り付けると楽