---
title: "Github Actions でzenn に予約投稿する方法まとめ"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Github Actions","Zenn","予約投稿"]
published: false
---


いくつかの記事を読んだのでそれぞれまとめる
1. branch を予約投稿分だけ作りpushする 
- `git checkout -b $name` `git push --set-upstream orgin $name`
- 次に`.github/workflows/~.yml`をブランチ分だけ作成してscheduleで指定した時刻にmergeするようにする
@[card](https://zenn.dev/j5c8k6m8/articles/zenn-adcal-github-action)

2. workflowでは指定せずに記事中に公開日時を指定しているっぽい
@[card](https://zenn.dev/kyoh86/articles/c02511740d70365a1420)

3. github actions のmergeで予約投稿をするのをわかりやすく解説したやつ
@[card](https://zenn.dev/ryo_kawamata/articles/schedule-publish-on-zenn-article)

4. ファイルを直接書き換える系
@[card](https://zenn.dev/korosuke613/articles/zenn-metadata-updater)
markdownファイルからmetadataを取得してそれを書き換える

