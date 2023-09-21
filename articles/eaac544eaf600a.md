---
title: "ローカルでのrebase後にコンフリクト解消してもpull, push出来ない"
emoji: "📓"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [git]
published: false
---

## conflict を解消

ローカルでの rebase 後にコンフリクト解消しても、リモートにpushができないケースが時々ある。その場合に --force はいささか乱暴なのでもう少し安全にやる方法を解説。

## 対応法

```bash
❯ git push --force-with-lease --force-if-includes
```

**`--force-with-lease`** オプションを指定すると、ローカルの ref とリモートの ref を比較しローカルが最新か判定。最新なら push し、そうでなければが失敗してくれるようになる。

ただし、**merge なしに fetch するな** もしくは **定期的にpull** を守っていれば、ローカルの ref が最新であることが保証されるが、うっかりミスは誰にでもある。

上記のような状態では、***--force-with-lease*** オプションをつけても、直前にfetchしていると push が成功してしまう。この対策として **`--force-if-includes`** オプションを併用するとよい。

上記のようなコマンドは長いので、

```bash
❯ git config --global alias.pushf 'push --force-with-lease --force-if-includes'
```

としておくと、`git pushf` で実行できるようになる;-)
