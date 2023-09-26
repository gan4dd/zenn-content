---
title: "git push の安全な実行：--force-with-lease と --force-if-includes の最適な組み合わせ"
emoji: "📓"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [git,tips]
published: true
---

## はじめに

git の使い方として、**merge なしに fetch するな** とか **定期的にpull** を行うことが推奨されている。これを守っていれば、ローカルの ref が最新であることが保証されるが、実際には、ローカルで作業しているときに、他の人が push してしまうことは多々発生する。

## git rebase 後の conflict 解消

ローカルで rebase 後にコンフリクト解消しても、リモートにpushができないケースが時々ある。
そんなケースで安易に「git push -f」（***--force*** ）を行うのはデグレの原因にもなるのでより安全な方法を紹介する。

## 対応法

解決法は push するときには次の2つのオプションをつける;-)

```bash
❯ git push --force-with-lease --force-if-includes
```


**`--force-with-lease`** オプションを指定すると、ローカルの ref とリモートの ref を比較しローカルが最新か判定。最新なら push し、そうでなければが失敗してくれるので、誤って古いファイルで上書きといった悲惨な状態を回避できるようになる。

**`--force-if-includes`** オプションは、reflog に remotes/origin/main (I) が含まれていることをチェックし、直前fetch状態なら拒否してくる仕組み。


***--force-with-lease*** オプションだけでは、直前にfetchしていると push が成功してしまうので、
***--force-if-includes*** オプションを併用すると更に安全に行えるというのがポイント。



## まとめ

ローカルで rebase 後にリモートにpushするときは、

```bash
❯ git push --force-with-lease --force-if-includes
```

しましょう。

なお、上記のようなコマンドは長いので、

```bash
❯ git config --global alias.pushf 'push --force-with-lease --force-if-includes'
```

としておくと、**`git pushf`** で実行できるようになるのでtype数が減って楽ができる;-)
