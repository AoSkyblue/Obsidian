---
title: "MacBookを閉じたときにスリープを防ぐ方法 (Monterey対応)"
source: "https://qiita.com/shge/items/ae2f725be8009f786122"
author:
  - "[[Qiita]]"
published: 2022-07-17
created: 2025-04-30
description: "MacBook の蓋を閉じたときも含め、すべてのスリープ機能を一時的に無効にする方法です。AC電源に接続していないときでもスリープしなくなってしまうので注意してください。※蓋を閉じたときではなく…"
tags:
  - "clippings"
image: "https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-user-contents.imgix.net%2Fhttps%253A%252F%252Fcdn.qiita.com%252Fassets%252Fpublic%252Farticle-ogp-background-afbab5eb44e0b055cce1258705637a91.png%3Fixlib%3Drb-4.0.0%26w%3D1200%26blend64%3DaHR0cHM6Ly9xaWl0YS11c2VyLXByb2ZpbGUtaW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnFpaXRhLWltYWdlLXN0b3JlLnMzLmFtYXpvbmF3cy5jb20lMkYwJTJGMTU1NTg3JTJGcHJvZmlsZS1pbWFnZXMlMkYxNTIyOTkyNjk0P2l4bGliPXJiLTQuMC4wJmFyPTElM0ExJmZpdD1jcm9wJm1hc2s9ZWxsaXBzZSZmbT1wbmczMiZzPTJmMWNiMzIxOTI3ZTYxOTUxZmEzZTE5Y2YzZjE4MzBl%26blend-x%3D120%26blend-y%3D467%26blend-w%3D82%26blend-h%3D82%26blend-mode%3Dnormal%26s%3Dfaaa35425d83dd86c1dde5baae2f8390?ixlib=rb-4.0.0&w=1200&fm=jpg&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTk2MCZoPTMyNCZ0eHQ9TWFjQm9vayVFMyU4MiU5MiVFOSU5NiU4OSVFMyU4MSU5OCVFMyU4MSU5RiVFMyU4MSVBOCVFMyU4MSU4RCVFMyU4MSVBQiVFMyU4MiVCOSVFMyU4MyVBQSVFMyU4MyVCQyVFMyU4MyU5NyVFMyU4MiU5MiVFOSU5OCVCMiVFMyU4MSU5MCVFNiU5NiVCOSVFNiVCMyU5NSUyMCUyOE1vbnRlcmV5JUU1JUFGJUJFJUU1JUJGJTlDJTI5JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LXBhZD0wJnM9ZDZkYjg2NGEzNzdhNGEwYjU2ODljMDdmOTkxMDM1NTA&mark-x=120&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTgzOCZoPTU4JnR4dD0lNDBzaGdlJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LXBhZD0wJnM9NThhZDM3OTg5YjcwNGQ3NWU5MzNjOTc2MmI0YWZjMzI&blend-x=242&blend-y=480&blend-w=838&blend-h=46&blend-fit=crop&blend-crop=left%2Cbottom&blend-mode=normal&s=ba78b4d17911789b07cddd641af861f3"
PermanentNote:
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=735078)

この記事は最終更新日から1年以上が経過しています。

MacBook の蓋を閉じたときも含め、すべてのスリープ機能を一時的に無効にする方法です。  
AC電源に接続していないときでもスリープしなくなってしまうので注意してください。

※蓋を閉じたときではなく通常の時間経過によるスリープを無効化したい場合は、システム環境設定の「省エネルギー」から行うことができます。

スリープを無効化中も、システム環境設定→「Dock」→「ホットコーナー」から設定することでディスプレイをスリープすることはできます。

## スリープを無効にする方法

1. 「アプリケーション」フォルダ→「ユーティリティ」フォルダにある「ターミナル」を開く
2. 以下のコマンドを入力し、Enterキーを押す

```shell
sudo pmset -a disablesleep 1
```

スリープが無効になっているかは、Apple メニューの「スリープ」が、グレーになってクリックできない状態になっているかどうかで判断できます。

## 元に戻す方法

上のコマンドの `1` を `0` にして実行します。

```shell
sudo pmset -a disablesleep 0
```

## ステータス確認

```shell
pmset -g
```

[![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/155587/2cba52ed-979b-c45a-5dfd-ff02527d748d.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F155587%2F2cba52ed-979b-c45a-5dfd-ff02527d748d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=88fc660b4d97f4117f3515e3d58a9307)

## クラムシェルモードについて

外付けディスプレイを接続していると蓋を閉じたときにスリープしない場合があります。その際はクラムシェルモードをオフにするとスリープするようにすることができます。  
復元モード (`Command+R`) で起動してユーティリティ→ターミナルから以下のコマンドを実行します。

```sh
nvram boot-args="niog=1"
```

## 参考

[https://support.apple.com/ja-jp/HT200106](https://support.apple.com/ja-jp/HT200106)  
[https://discussionsjapan.apple.com/thread/110190796](https://discussionsjapan.apple.com/thread/110190796)

## エイリアス

`~/.bashrc` ファイルに以下を書き込むことで `sleepon` `sleepoff` で無効化/有効化が可能になります。

~/.bashrc

```shell
alias sleepon='sudo pmset -a disablesleep 0'
alias sleepoff='sudo pmset -a disablesleep 1'
```

[2](https://qiita.com/shge/items/#comments)

コメント一覧へ移動

[80](https://qiita.com/shge/items/ae2f725be8009f786122/likers)

いいねしたユーザー一覧へ移動

69