---
title: Amazon VPCを「これでもか！」というくらい丁寧に解説
source: https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77
author:
  - "[[Qiita]]"
published: 2022-10-14
created: 2025-03-31
description: はじめにAWS上で仮想ネットワークを構築できるAmazon VPCは、多くのAWSサービスが動作する基盤となる、非常に重要かつ多機能なサービスです。多機能ゆえに公式ドキュメントやネット上の記事も…
tags:
  - clippings
image: https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-user-contents.imgix.net%2Fhttps%253A%252F%252Fcdn.qiita.com%252Fassets%252Fpublic%252Farticle-ogp-background-afbab5eb44e0b055cce1258705637a91.png%3Fixlib%3Drb-4.0.0%26w%3D1200%26blend64%3DaHR0cHM6Ly9xaWl0YS11c2VyLXByb2ZpbGUtaW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnFpaXRhLWltYWdlLXN0b3JlLnMzLmFwLW5vcnRoZWFzdC0xLmFtYXpvbmF3cy5jb20lMkYwJTJGNjEwMTY3JTJGcHJvZmlsZS1pbWFnZXMlMkYxNTg1NDExMDg5P2l4bGliPXJiLTQuMC4wJmFyPTElM0ExJmZpdD1jcm9wJm1hc2s9ZWxsaXBzZSZmbT1wbmczMiZzPTFhNmQ4ODA1Y2IwMzMyMmFkOTMzY2Y4YzNmNjcyYmM4%26blend-x%3D120%26blend-y%3D467%26blend-w%3D82%26blend-h%3D82%26blend-mode%3Dnormal%26s%3D93567732de223a1733c41cd6d88e21aa?ixlib=rb-4.0.0&w=1200&fm=jpg&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTk2MCZoPTMyNCZ0eHQ9QW1hem9uJTIwVlBDJUUzJTgyJTkyJUUzJTgwJThDJUUzJTgxJTkzJUUzJTgyJThDJUUzJTgxJUE3JUUzJTgyJTgyJUUzJTgxJThCJUVGJUJDJTgxJUUzJTgwJThEJUUzJTgxJUE4JUUzJTgxJTg0JUUzJTgxJTg2JUUzJTgxJThGJUUzJTgyJTg5JUUzJTgxJTg0JUU0JUI4JTgxJUU1JUFGJUE3JUUzJTgxJUFCJUU4JUE3JUEzJUU4JUFBJUFDJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LXBhZD0wJnM9NTIzMmU0Y2E2MzE5ODVjMmYzMTFmZDk5YmQxMWU0NjE&mark-x=120&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTgzOCZoPTU4JnR4dD0lNDBjNjBldmFwb3JhdG9yJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LXBhZD0wJnM9YjAyNzE3YzhlNDQ1YjRlMDZhNzEzMGZkMjA4ZGJmYzI&blend-x=242&blend-y=480&blend-w=838&blend-h=46&blend-fit=crop&blend-crop=left%2Cbottom&blend-mode=normal&s=d0dbec5cd32be677752fc79239546f57
PermanentNote: "[[2025-03-31_07-52-41]]"
---
info

この記事は最終更新日から1年以上が経過しています。

[@c60evaporator (Kenta Nakamura)](https://qiita.com/c60evaporator) 最終更新日 投稿日 2022年08月07日

## はじめに

**AWS上で仮想ネットワークを構築** できる **Amazon VPC** は、多くのAWSサービスが動作する基盤となる、 **非常に重要かつ多機能** なサービスです。

多機能ゆえに [公式ドキュメント](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/what-is-amazon-vpc.html) やネット上の記事も断片的な機能の解説が多く、 **全体像を把握することが難しい** サービスとも言えます。

[![VPCの機能の多さ](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Feb3f439d-d8b7-8c2a-ea36-37999f641d5e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e35b0b4dcb3756520e6c47d83005855d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Feb3f439d-d8b7-8c2a-ea36-37999f641d5e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e35b0b4dcb3756520e6c47d83005855d)

そこで本記事は **VPCの全体像を理解できるよう、各機能のつながりや動作原理を丁寧に解説し** 、

「 **VPC界の百科事典** 」 (あくまで例えですが…笑)

[![VPC界の百科事典](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F189a368a-4740-8271-147d-031b096c8f0e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8dfdb7707a44b144e42bca104adb439c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F189a368a-4740-8271-147d-031b096c8f0e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8dfdb7707a44b144e42bca104adb439c)

となるような記事を目指したいと思います。

## 【追記】 実践編の記事を追加しました

**VPCの実画面での構築方法** は、 **以下の別記事にまとめました** 。「VPCを実際に触ってみたい！」という方は、こちらも **ご一読** いただけると嬉しいです。

## VPCとは

「Virtual Private Cloud」の略で、クラウド上に **仮想的なネットワークを構築** するためのサービスです。

例えば、オンプレ環境でWebアプリ(3層アーキテクチャ)と社内システムを運用するために、以下のようなネットワークを構築していたとします。

[![オンプレネットワーク](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fe754327a-c347-82fc-ec1c-cdc4d9e42da6.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=75fdc9358cf1a84b2c6d155bcd4e375f)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fe754327a-c347-82fc-ec1c-cdc4d9e42da6.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=75fdc9358cf1a84b2c6d155bcd4e375f)

これを **VPC上の仮想ネットワークで代用** すると、以下のようになります（※ [こちらの公式ドキュメントの構成](https://aws.amazon.com/jp/blogs/news/how-to-think-about-zero-trust-architectures-on-aws/) を参考にした一例です）

[![オンプレのVPCによる置換](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F42d9caf9-082f-9340-3cb7-490628d5938e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=90bf4155a0af9ca7df55a9399c66f9d9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F42d9caf9-082f-9340-3cb7-490628d5938e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=90bf4155a0af9ca7df55a9399c66f9d9)

上図を見ての通り、 **オンプレでのネットワーク構築と** 設計思想が **似ている部分** (例: DMZとパブリックサブネットが対応している)と **異なる部分** (例: AWSにはS3やRoute53のようなVPCネットワーク外のサービスが存在)があり、セキュアかつ高性能なシステムを構築するためにはVPC独自の機能を広く理解する必要があります。

なお、 **VPC内に設置** するサービスと、 **VPC外部に存在** する **サービスの一覧** は、以下の記事が参考になります。

図を見るとLambdaやS3はVPCの外にあるように見えますが、Lambdaは設定次第でVPC内に設置することができますし、S3使用時も 後述のVPCエンドポイント 経由で他のサービスからアクセスする事が多いです。

また図には記載されていないFargateもVPC内に設置するサービスであり、 **AWSの主要サービスの多くがVPCと密接に関連している** 事が分かります。

  

## VPCのメリット

多くのAWSサービスはネットワークを構築しなくとも個別に通信する事ができますが、この方式では成り行きで通信経路が決まってしまうため、セキュリティや性能面での制御が難しくなってしまいます。  
これに対し **VPCを利用** することで、以下のようなメリットを得る事ができます。

- 通信経路を絞って監視・アクセス制御を行うことで、 **セキュリティを向上** させる
- 通信経路を明示的に制御してロードバランサ等でスケールアウトすることで、 **性能を担保** できる
- 通信経路を明示的に複数確保することで、 **可用性を向上** させる事ができる
- システム内のサービスを整理して一括管理する事で、 **運用・保守性を向上** させる事ができる
- IaCサービスと組み合わせることで、 **移行性を向上** させる事ができる

これらの項目をまとめると、 **VPCの利用** が「 **非機能要件** 」 **の担保** ( [後述](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%AE%E5%BD%B9%E5%89%B2) )に大きく寄与することがわかります。

## VPCの役割

システム・アプリ開発においてネットワーク設計は、以下の **非機能要件** (※ [IPAの定義](https://www.ipa.go.jp/files/000005076.pdf) を参照)を満たす上で大きな役割を果たします。

**1\. 可用性** → 必要なときにいつでもサービスが提供されており、停止していない  
**2\. 性能・拡張性** → 必要な速度が確保されている＆利用者が増えても対応できる  
**3\. 運用・保守性** → 運用監視や問題発生時の対応を適切に行える  
**4\. 移行性** → 新環境への移行の容易性  
**5\. セキュリティ** → 不正アクセスを防止する  
**6\. システム環境・エコロジー** → システムの設置環境や、自然環境に与える影響

### オンプレにおけるネットワーク設計の難しさ

**オンプレ環境** においては、上記要件を満たすために **高度なネットワーク機器の知識** および **機器を揃えるためのコスト** が必要となります。  
例えば可用性を向上させるためには、サーバや回線を冗長化した上で、自動でバックアップに切り替えるための **設定** (下図の [リンクアグリゲーション等](https://bizdrive.ntt-east.co.jp/articles/dr00101-013.html) )を実施する必要があります。  
[![linkaggregation.jpg](https://qiita-user-contents.imgix.net/https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2F5%2F5b%2FLink_Aggregation1.JPG?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d22c131ac434a718836a49e2e8062cc0)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2F5%2F5b%2FLink_Aggregation1.JPG?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d22c131ac434a718836a49e2e8062cc0)  
上記の専門知識、コストが、 **一般エンジニアや小規模組織が適切な非機能要件を実現するためのハードル** となり、結果不十分な対策によりシステム障害やセキュリティ事故に繋がることも多々あります。

### AWSのネットワーク設計上のメリット

一方で **AWSでは** 、 [可用性やセキュリティ向上のための施策がデータセンタに組み込まれているため](https://aws.amazon.com/jp/compliance/data-center/controls/) 、デフォルト状態でもある程度高い非機能要件が確保されています。

加えて、さらに **非機能要件を向上させるためのサービス** が多数準備されており、これらを組み合わせるだけで高度な非機能要件を実現できてしまいます。

よって専用機器の購入や高度なハードの知識が不要となるため、 **小規模組織でも適切な非機能要件を実現しやすく** なります。

**VPC** はこれらのサービス同士を **ネットワークで繋ぐ基盤** となるため、可用性やセキュリティ向上のために正しい知識の会得は不可欠と言えます。

### AWSにおけるネットワーク関連サービス一覧

参考までに、 **ネットワーク上で非機能要件を向上させるための主なAWSサービス** を列挙します。

| サービス名                                                               | 該当する非機能要件                     | 概要                                                        |     |
| ------------------------------------------------------------------- | ----------------------------- | --------------------------------------------------------- | --- |
| [**ELB**](https://aws.amazon.com/jp/elasticloadbalancing/)          | 1.可用性,   2.性能・拡張性             | ロードバランサによる過負荷防止                                           |     |
| [**Cloud Front**](https://aws.amazon.com/jp/cloudfront/)            | 1.可用性,   2.性能・拡張性             | CDNによるアクセス速度向上と過負荷防止                                      |     |
| [**Route 53**](https://aws.amazon.com/jp/cloudfront/)               | 1.可用性,   2.性能・拡張性,   5.セキュリティ | ヘルスチェックによる可用性向上、DNSラウンドロビンによる拡張性向上、DNSファイアウォールによるセキュリティ向上 |     |
| [**Network Firewall**](https://aws.amazon.com/jp/network-firewall/) | 5\. セキュリティ                    | サブネットに対するアクセスルールを設定する                                     |     |
| [**WAF**](https://aws.amazon.com/jp/waf/)                           | 5\. セキュリティ                    | Webアプリに対する一般的な攻撃パターンをブロックする                               |     |
| [**Direct Connect**](https://aws.amazon.com/jp/waf/)                | 5\. セキュリティ                    | 専用線接続によるセキュリティ向上                                          |     |
| (おまけ:[AWSと環境負荷](https://aws.taf-jp.com/blog/50636) )                | 6\. システム環境・エコロジー              | AWS等の大規模クラウドプロバイダは世界平均よりも炭素強度が28%低い電源構成を使用しているとの事         |     |

これらのサービスは [後述のVPC機能紹介](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%AE%E4%BB%95%E7%B5%84%E3%81%BF%E3%81%A8%E4%B8%BB%E8%A6%81%E6%A9%9F%E8%83%BD) で頻繁に登場する事から分かるように、基本的に **VPCと連動した動作** が前提となっています。

上記から、 **AWSのネットワーク構築および非機能要件担保** において、 **VPCは基盤となる重要なサービス** であることが分かります。

## VPCの仕組みと主要機能

VPCの仕組みと主要機能について、以下の項目に分けて解説します。

- [**ネットワークの基礎知識**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%81%AE%E5%9F%BA%E7%A4%8E%E7%9F%A5%E8%AD%98)
- [**VPCの構造**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%AE%E6%A7%8B%E9%80%A0)
- [**ルートテーブル**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%AB%E3%83%BC%E3%83%88%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB)
- [**VPCと外部ネットワーク・サービスとの接続**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E5%A4%96%E9%83%A8%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%A8%E3%81%AE%E6%8E%A5%E7%B6%9A)
- [**VPN**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpn-%E4%BB%AE%E6%83%B3%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF)
- [**VPCのセキュリティ機能**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%AE%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E6%A9%9F%E8%83%BD)
- [**IPアドレス管理用サービス**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#ip%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9%E7%AE%A1%E7%90%86%E7%94%A8%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9)
- [**VPCとログ**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%83%AD%E3%82%B0)

## ネットワークの基礎知識

既にご存知の内容も多いかと思いますが、VPCを理解するためにはネットワークの知識が必要となるため、簡単に解説します。

### IPアドレスとは

ネットワークの階層構造を表すOSI参照モデルにおいて、ネットワーク層で **通信相手を識別する際に使用される番号** です。

| 階層 | 名称 | 通信相手の識別に使用する情報 |
| --- | --- | --- |
| 第7層 | アプリケーション層 | プロトコルにより異なる |
| 第4層 | トランスポート層 | ポート番号 |
| 第3層 | **ネットワーク層** | **IPアドレス** |
| 第2層 | データリンク層 | MACアドレス |
| 第1層 | 物理層 | 物理的な接続 |

※第5層(セッション層)、第6層(プレゼンテーション層)はTCP/IPではアプリケーション層と一体化されるため省略します

ネットワーク層は主に広域での通信で主要な役割を果たすため、IPアドレスを **インターネットおよび組織ネットワーク上での住所** とみなすと、理解しやすいかと思います。

### IPアドレスの構造

IPアドレスには32ビットの数値で表される「IPv4」と、128ビットの数値で表される「IPv6」が存在しますが、2022年時点ではIPv4が主に使用されます

IPv4のIPアドレスは **32ビット** の数値で表され、10進数では以下のように8ビットごとにドットで区切られて表現されます。

|  | 10進数での表記 | 2進数での表記 |
| --- | --- | --- |
| **例1** | 192.168.1.2 | 11000000 10101000 00000001 00000010 |
| **例2** | 8.8.8.8 | 00001000 00001000 00001000 00001000 |

### グローバルIPアドレスとプライベートIPアドレス

#### ・グローバルIPアドレス

**世界中で一意に定められる** IPアドレスを「 **グローバルIPアドレス** 」と呼びます。

グローバルIPアドレスはインターネット経由でのアクセスに用いられ、文字通りインターネット上での「住所」に相当するアドレスとなります。

**AWSにおいて** は、グローバルIPアドレスは「 **パブリックIPアドレス** 」と呼ばれる事も多いです。

#### ・プライベートIPアドレス

**限られた範囲内** で用いられ、その範囲内のみ一意に定める必要がある(範囲外では重複が許される)IPアドレスを、「 **プライベートIPアドレス** 」と呼びます。

「限られた範囲」は、一般的に組織が相当することが多いため、組織内のネットワークをビルに例えた時の「部屋番号」とみなすと理解しやすいかと思います。

どのようなアドレスがプライベートIPアドレスとなり、どのようなアドレスがグローバルIPアドレスとなるかは、 [後述の「使用可能なIPアドレス」](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E4%BD%BF%E7%94%A8%E5%8F%AF%E8%83%BD%E3%81%AAip%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9) を参照ください

### ネットワーク部とホスト部

IPアドレスは、 **ネットワーク部** と **ホスト部** に分けられます。

32ビットのIPv4アドレスであれば、上位xビットのネットワーク部と、下位32-xビットのホスト部に分かれ、例えばIPアドレス `192.168.1.2` においてネットワーク部が24ビットであれば、

|  | 10進数表記 | 2進数表記 |
| --- | --- | --- |
| IPアドレス全体 | `192.168.1.2` | 11000000 10101000 00000001 00000010 |
| ネットワーク部 | `192.168.1` の部分 | 11000000 10101000 00000001 |
| ホスト部 | `2` の部分 | 00000010 |

となります。

また、 **ネットワーク部を1、ホスト部を0** ビット表記したものを **サブネットマスク** と呼びます。  
例えばネットワーク部が24ビットであれば、サブネットマスクは `255.255.255.0` (2進数では `11111111 11111111 11111111 00000000` )となります

ネットワーク部の長さは、 **サブネットマスク長** または **プレフィックス長** と呼ばれる事もあります。

またホスト部を全て0で埋めたアドレスは **ネットワークアドレス** と呼ばれ、 [後述](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%B5%E3%83%96%E3%83%8D%E3%83%83%E3%83%88) のサブネット自身を表すアドレスとして使用されます。

### サブネット

**ネットワーク部が等しいIPアドレス範囲** のことを **サブネット** と呼び、サブネットを表すアドレスを **ネットワークアドレス** と呼びます  
(サブネットのことを単に「ネットワーク」と呼ぶこともあります)

**サブネット外へのアクセス** には **ルーティングが必要** となるため、ルーティングテーブル( [後述](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0) )やファイアウォール(こちらも [後述](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A8acl) )によるアクセス制御は基本的にサブネット単位で行われ、 **ホストのグルーピングやセキュリティ向上** のためにサブネットは重要な要素となります。

### CIDR (Classless Inter-Domain Routing)

通常、プライベートIPアドレスのプレフィックス長は、 [後述のクラスA～C](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E4%BD%BF%E7%94%A8%E5%8F%AF%E8%83%BD%E3%81%AAip%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9) の固定値(8ビット、16ビット、24ビット)が使用されます。  
これだけでは3通りのプレフィックス長しか使用できないので、ホスト数によっては **アドレス空間が大幅に余って** しまい、無駄の多いネットワークとなってしまいます。

そこで、 **上記3通り以外のプレフィックス長を動的に設定できる** ようにした **CIDR** (Classless Inter-Domain Routing、「サイダー」と読みます)と呼ばれる機構が広く用いられています。

CIDRにおいてネットワークアドレスを表す記法を **CIDR記法** と呼び、例えば `192.168.1.2` においてネットワーク部が26ビットであれば、ネットワークアドレスは `192.168.1.0/26` のように表記します。

### 使用可能なIPアドレス

IPアドレスの指定には [ICANN](https://www.icann.org/) という組織がルールを定めており、用途に応じて使用できるアドレス範囲が決められています。

**VPCでプライベートIPアドレスとして使用できる** のは、 **以下太字のアドレス範囲** のみです。  
下記の範囲内から作成するサブネットのネットワークアドレスを決定してください。

| IPアドレスの範囲 | 用途 |
| --- | --- |
| 127.0.0.0〜127.255.255.255 | ループバックアドレス (自分自身を指すアドレス) |
| 169.254.0.0〜169.254.255.255 | リンクローカルアドレス (ルータを経由せずにアクセスするためのアドレス) |
| 224.0.0.0〜239.255.255.255 | マルチキャストアドレス (予め定められたルールで複数のアドレスに同時送信する際に使用) |
| 255.255.255.255 | 制限ブロードキャストアドレス |
| **10.0.0.0〜10.255.255.255** | **プライベートアドレス** (クラスA=プレフィックス長8bit) |
| **172.16.0.0〜172.31.255.255** | **プライベートアドレス** (クラスB=プレフィックス長16bit) |
| **192.168.0.0〜192.168.255.255** | **プライベートアドレス** (クラスC=プレフィックス長24bit) |
| 上記以外 | グローバルアドレス等 |

また、 **AWSにおいて以下のホスト部はAWS側で利用** されているため、ユーザ側が新たに **ホストに設定することはできません** 。

| ホスト部 | 用途 | ネットワークアドレスが10.0.1.0/28のときの例 |
| --- | --- | --- |
| 0 | ネットワークアドレスを表す | 10.0.1.0 |
| 1 | VPCルータ | 10.0.1.1 |
| 2 | Amazonが提供するDNSサービス | 10.0.1.2 |
| 3 | AWSで予約されているアドレス | 10.0.1.3 |
| 全ビットが1 | ブロードキャストアドレス | 10.0.1.15 |

### NAT

NATとNAPTについて解説します。

#### ・NAT

**グローバルIPアドレスとプライベートIPアドレスを変換** する機構を、 **NAT** (Network Address Translation)と呼びます。

**・NATによる変換例**

| グローバルIPアドレス | プライベートIPアドレス |
| --- | --- |
| a.b.c.d | 192.168.0.1 |
| a.b.c.e | 192.168.0.2 |

通常 **インターネット上から** プライベートIPアドレスにアクセスする事は出来ませんが、 **NATを使用する事** により、グローバルIPアドレス宛に届けられたパケットの宛先をプライベートIPアドレスに変換する事で、 **プライベートIPアドレスが設定されたホストにアクセス** することができます。

#### ・NAPT

NATはIPアドレスのみを変換対象としますが、 **IPアドレスに加えてポートも変換対象とする** 機構を、 **NAPT** (Network Address Port Translation)と呼びます。

多くの一般家庭ではグローバルIPアドレスは1個しか割り振られないため、NATでは1個のプライベートIPアドレスしかインターネットからアクセスできませんが、 **NAPTを使用** する事で **1個のグローバルIPアドレスに複数のプライベートIPアドレスを紐づけられる** ため、複数のプライベートIPアドレスにインターネットからアクセスする事ができます。

**・NAPTによる変換例**

| グローバルIPアドレス | グローバル側ポート番号 | プライベートIPアドレス | プライベート側ポート番号 |
| --- | --- | --- | --- |
| a.b.c.d | 80 | 192.168.0.1 | 80 |
| a.b.c.d | 81 | 192.168.0.2 | 80 |

なお、NATとNAPTを合わせて(広義の)NATと呼ぶことも広く行われています。

### ルーティング

**ルーティング** とは、サブネットの結節点において **他のサブネットとの通信経路を決定する制御** を指します。  
**自サブネット以外との通信にはルーティングが必要** となるため、複数のサブネットが存在するネットワークにおいて必須の技術と言えます。

ルーティングのルールを定めるテーブルを **ルーティングテーブル** と呼び、宛先IPアドレスとルーティングテーブルに基づいてどのサブネットにパケットが送信されるかが決定されます。  
ルールの決め方には、IPアドレスに基づいて **毎回同じ経路** が選択される「 **静的ルーティング** 」と、 **動的に経路が決定** される「 **動的ルーティング** 」が存在しますが、VPCでは基本的に静的ルーティングを使用します (VPNやトランジットゲートウェイ使用時は [BGPによる動的ルーティングでオンプレネットワークと連携](https://dev.classmethod.jp/articles/control-bgp-route-on-site-to-site-vpn/) することもできます)

#### ・ルーティングテーブルの見方

ルーティングテーブルは、 **ロンゲストマッチアルゴリズム** と呼ばれる方法に基づいてパケットの送信先を決めます。  
ロンゲストマッチアルゴリズムとは、送信したい宛先IPアドレスとルーティングテーブルの宛先ルート(一般的に複数登録されている)を比較し、 **宛先アドレスをサブネット内に含む宛先ルートの中で、最もプレフィックス長が長いものを選択** する手法です(詳細は後述)。

選択された宛先ルートと同一行を参照し、主に **インターフェイス** (パケットが送出されるルーターのポート)と **ネクストホップ** (次に転送されるルータ)からパケットの送信先が決定されます。

例として以下のようなオンプレネットワークを考えます。

[![ルーティングテーブルの見方](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F53ff95c6-d696-ba9f-75ab-ce17bffe0708.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7fbaa22c222e240f773d6b8282c5c489)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F53ff95c6-d696-ba9f-75ab-ce17bffe0708.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7fbaa22c222e240f773d6b8282c5c489)

**ルータ1のルーティングテーブル** が以下のように設定された場合を考えると

| 宛先ルート         | インターフェイス   (パケットが送出されるルーターのポート) | ルーティング方法       | ネクストホップ   (次に転送されるルータ) |
| ------------- | ------------------------------- | -------------- | ---------------------- |
| 172.31.1.0/24 | 0                               | 直接接続           | \-                     |
| 172.31.2.0/24 | 1                               | 直接接続           | \-                     |
| 172.31.3.0/24 | 1                               | 静的ルーティング       | 172.31.2.2             |
| 0.0.0.0/0     | 1                               | 動的ルーティング(OSPF) | 動的ルーティングで決定            |

例えば宛先アドレスが `172.31.3.4` の場合、このアドレスがサブネット内に含まれる宛先ルートには `172.31.3.0/24` と `0.0.0.0/0` の2つの候補が存在しますが、よりプレフィックス長の長い前者が選択され、静的ルーティングに基づいてインターフェイス(IF)1からパケットが送出され、ネクストホップに基づいて `172.31.2.2` のルータ2に転送されます（これ以降の経路はルータ2のルーティングテーブルに基づいて決定されます）

また、もう一例として宛先アドレスが `8.8.8.8` の場合を考えると、このアドレスがサブネット内に含まれる宛先ルートは `0.0.0.0/0` に絞られ、IF1からパケットが送出されます。この行では動的ルーティングが選択されているため、ネクストホップをルータ3にするかルート4にするかは、ルーティング方法(OSPF,RIP,BGP等)や経路内の障害情報に基づいて動的に選択されます。

難しいですね。実際、オンプレネットワークにおいてルーティングの設定は高度なスキルが必要とされる分野の一つであり、 **VPCではよりシンプルなルーティング設定** が実現できることも、 **クラウドを使用するメリットの一つ** となります。

#### ・オンプレとVPCのルーティングの違い

オンプレネットワークにおいては、サブネット間の結節点である「ルータ」がルーティングを実施します。  
一方で **VPC** においては、 **「ルートテーブル」というサービス** を使用する事で [**サブネット** (後述)](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%82%B5%E3%83%96%E3%83%8D%E3%83%83%E3%83%88) **ごとにルーティングを制御** できます。

VPCではルータ同士の物理的な位置関係や、動的ルーティングを考慮しなくとも良いので、 **オンプレよりもシンプルなルーティング** を実現できます

### DHCP

**DHCP** (Dynamic Host Configuration Protocol)とは、ホストにネットワーク関係の情報を渡すためのプロトコルです。

一般的に、ホストに **プライベートIPアドレスを動的に割り振る** 際に使用されますが、それ以外にもDNSサーバのIPアドレスやデフォルトゲートウェイ(ルーター)のIPアドレス等の情報がホストに渡されます。

業務向けシステムではDHCP機能を実現するDHCPサーバが置かれる事が一般的ですが、一般家庭で使用されるルータにもこのDHCP機能が標準装備されていることが多く、これによりユーザが特別な設定をしなくとも、プライベートIPアドレスが割り振られてネットワーク通信ができたり、DNSによる名前解決でドメイン名(URL)からホームページにアクセスする事が出来ます。

VPCにおいても、後述の [**Elastic IP**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#elastic-ip) **を使用しない限り** はこの **DHCP機能** が働き、 **VPC内部のインスタンスのIPアドレスは動的に割り振られ** ます (再起動するとIPアドレスが変わる)

なお、名前が似ている [DHCPオプションセット](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#dhcp%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%BB%E3%83%83%E3%83%88) は本来のDHCPの主機能であるIPアドレスの振り分けには使用されれず、DNSサーバやNTPサーバ等の情報をホストに渡す機能のみを持つ事にご注意ください。

### 外向き通信と内向き通信

本記事では「外向き」と「内向き」という言葉が頻繁に登場しますが、

外向き：内部ネットワーク(VPC等)からインターネット方向への通信  
内向き：インターネットから内部ネットワーク(VPC等)方向への通信

を表しています。

注意すべきが、インターネットで最も一般的に用いられるTCP通信では、下図のように基本的に要求と応答のセットで通信が行われるため、基本的に片方向のみで通信が完結する事はないということです。

[![TCP3ウェイハンドシェイク](https://qiita-user-contents.imgix.net/https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2Fthumb%2Ff%2Ff0%2FThree-way-handshake-example.gif%2F750px-Three-way-handshake-example.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=abe4b6003a1c6510c206779c9836b269)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2Fthumb%2Ff%2Ff0%2FThree-way-handshake-example.gif%2F750px-Three-way-handshake-example.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=abe4b6003a1c6510c206779c9836b269) ※画像はWikipediaより

このような双方向の通信を扱う際に、「 **ステートレス** 」と「 **ステートフル** 」の2種類の考え方が存在します。

#### ・ステートレス

パケットが要求であるか応答であるかは考慮せず、通信の方向のみで外向きか内向きかを判定する方法を **ステートレス** と呼びます。

#### ・ステートフル

**通信の起点となる要求パケット** (SYNパケット)の方向 **が外向き** なら、それに **付随する応答パケットも全て外向き** として扱う方法を、 **ステートフル** と呼びます

**ステートフル** は **最初の要求パケットの方向のみ** を考慮すれば良いため、 [後述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A8acl) ファイアウォールの **設定をシンプル** にすることができます。よって多くのファイアウォールではステートフルな設定を採用しています。

### ファイアウォールとACL

ファイアウォールは、 **特定のルールに基づいてネットワーク上の通信を制限** するための機構で、通信を制限するためのルールを記述したリストを **ACL** (Access Control List)と呼びます。

ACLではIPアドレスとポート番号に基づき通信を制限する事が一般的で、特定の条件のみ通信を許可し、それ以外を全て拒否する「 **ホワイトリスト** (Allow)」式と、特定の条件のみ通信を拒否し、それ以外を全て許可する「 **ブラックリスト** (Deny)」式が存在します。

また [前述のように](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E5%A4%96%E5%90%91%E3%81%8D%E9%80%9A%E4%BF%A1%E3%81%A8%E5%86%85%E5%90%91%E3%81%8D%E9%80%9A%E4%BF%A1) 通信方向の評価には、行きの **通信と戻りの通信を別々に評価** する **ステートレス** 方式と、 **行きと戻りをまとめて評価** する **ステートフル** 方式が存在します。

一般的にはセキュリティ強度の高い **ホワイトリスト式** 、かつ設定が容易でミスを防ぎやすい **ステートフル方式** が **よく用いられ** ており、 **VPCでも** この方式を採用した [「 **セキュリティグループ** 」](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%82%B0%E3%83%AB%E3%83%BC%E3%83%97) の **使用が推奨** されています。

この方式でのACLの例を下記します。

**・ACLの例**

| 許可するIPアドレスの範囲 | 許可するポート | 許可する方向 | (想定する通信) |
| --- | --- | --- | --- |
| 0.0.0.0/0 | 443 | 内向き | (HTTPS) |
| 192.168.0.0/24 | 22 | 内向き | (SSH) |
| 0.0.0.0/0 | 80 | 外向き | (HTTP) |
| 0.0.0.0/0 | 443 | 外向き | (HTTPS) |

上例では任意のIPアドレスからポート443番への内向きアクセス、サブネット192.168.0.0/24内のホストからポート22番への内向きアクセス、任意のIPアドレスへのポート80番、443番への外向きアクセスを許可し、それ以外の通信を遮断しています。  
(一般的には外向きアクセスの方が許可を緩めに設定し、外向きは全ての通信を許可しているケースも多いですが、近年は外向き通信を使用したサイバー攻撃用サーバ(C&Cサーバ)との通信による被害が増えているため、外向き通信にも制限を設けるケースが増えています)

また、ファイアウォールが [前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#nat) NATを兼ねている事も多く、この場合両者をまとめてインターネットとプライベートネットワークとの結節点に設置されます。

  
  

## VPCの構造

VPCの基本的な構造を解説します

### リージョンとAZ

AWSのデータセンターは、東京、バージニア州、シンガポール等の広域的な位置を表す「 **リージョン** 」と、リージョン内で地理的に一定距離(数km前後)離れた複数のデータセンターを表す「 **AZ** 」(アベイラビリティゾーン)から構成されます。

VPCで [**非機能要件**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%AE%E5%BD%B9%E5%89%B2) **を向上させる** ためには、この **リージョンとAZによる物理的位置の考慮が非常に重要** となるため、それぞれ解説します。

#### ・リージョン

データが実際に保存される地理的な位置を表します。  
全世界に数十個のリージョンが存在しますが、日本からの使用頻度が高いリージョンは以下となります。

**・主なリージョン**

| リージョン名 | 英名 | 場所 | 特徴 |
| --- | --- | --- | --- |
| アジアパシフィック (東京) | ap-northeast-1 | 日本 東京都 | 日本国内からの利用では第一の選択肢となる |
| アジアパシフィック (大阪) | ap-northeast-3 | 日本 大阪府 | 西日本からの通信遅延（レイテンシ）が少ないが、機能が限定されている（例：Transfer Accelerationが使用不可） |
| アジアパシフィック (シンガポール) | ap-southeast-1 | シンガポール | アジアで最初のリージョン |
| 米国東部 (バージニア北部) | us-east-1 | 米国 バージニア州 | 世界で最初のリージョン、費用が安めでサービスが豊富。日本からは通信遅延大 |

当然ながら、異なるリージョン間での通信では遅延(RTT)が大きくなり、また [データ転送コストも増える](https://aws.amazon.com/jp/ec2/pricing/on-demand/#Data_Transfer) ため、基本的には需要地と近いリージョンを選択する事が両者の利便性向上に繋がります。

#### ・ アベイラビリティゾーン（AZ）

1つのリージョンは、実は地理的に離れた複数のデータセンターから構成されています。  
実際の位置は公開されていませんが、例えば品川区と江東区にサーバが分散されているようなイメージです。

この個々のデータセンターのことをアベイラビリティゾーンを呼び、複数のアベイラビリティゾーンを跨いだ構成（ **マルチAZ** ）とすることで、リージョンを跨いだ場合と同様に **可用性向上** およびデータの消失対策に繋げられます。

[VPCの **ベストプラクティス**](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/vpc-security-best-practices.html) でも **マルチAZ構成による可用性向上が推奨** されており、 [冒頭のVPC構成例](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%81%AF) でもマルチAZによる並列なネットワーク構築を実施しています。具体的な構築法は 別記事(作成中) で解説します。

  

### VPCとその上限個数

1つのAWSアカウントには複数のVPCを作成する事ができ、それぞれのVPCは以下の条件を満たす必要があります

- VPCは **リージョンをまたげない** (VPCを作成するリージョンを一意に指定する必要あり)
- VPCは **AZをまたげる** (AZをまたぐ事でマルチAZ構成を実現できる)
- 1アカウントで **1リージョンあたり最大5個までVPCを作成可能** (申請すれば6個以上も可能)
[![VPCとリージョンとAZ](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F9c219c92-c835-2073-b6f9-a77878a6c1e3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1d225acb610a6486829d2327cd4d2ce8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F9c219c92-c835-2073-b6f9-a77878a6c1e3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1d225acb610a6486829d2327cd4d2ce8)

VPCには、以下のように16～28ビットのプレフィックス長を用いることができます

- 最小サイズ → /28 (11ホスト)
- 最大サイズ → /16 (65531ホスト)

また、以下の「デフォルトVPC」がアカウント作成時にデフォルトで作成されます。

#### ・デフォルトVPC

**AWSアカウント作成時** には、リージョンごとにVPCが1個自動的に作成されます。  
これを「 **デフォルトVPC** 」と呼び、ネットワークアドレス172.31.0.0/16が割り当てられています。  
デフォルトVPC内には

- AZごとに1個 (バージニア北部リージョンは計6個、それ以外は計3個)のパブリックサブネット
- 1個のセキュリティグループ
- 1個のルートテーブル
- 1個のインターネットゲートウェイ
- 1個のネットワークACL
- 1個のDHCPオプションセット

が予め作成されています

### VPCとサブネット

VPC内部には、以下のように複数のサブネットを作成する事ができます。

[![VPCとサブネット](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F0a2939b1-3fa0-5fd2-16eb-a7f0a1e20902.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=786d32d100d30018d7eec48eab8a869b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F0a2939b1-3fa0-5fd2-16eb-a7f0a1e20902.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=786d32d100d30018d7eec48eab8a869b)

この **VPCとサブネットの階層構造** が、VPCにおける **基本的なグルーピング単位** となります。

サブネットには [VPCに設定したプレフィックス長](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%81%9D%E3%81%AE%E4%B8%8A%E9%99%90%E5%80%8B%E6%95%B0) よりも長いプレフィックス長を設定することで、VPC内に複数のサブネットを格納することができます

[![サブネットのプレフィックス長](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Ffa570dc5-8615-d639-a5c3-226e4094a3b0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ab97b6eea4fff84a19dc73855761f6a6)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Ffa570dc5-8615-d639-a5c3-226e4094a3b0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ab97b6eea4fff84a19dc73855761f6a6)

このサブネット内に、EC2やRDS、Fargateのような各種サービス (インスタンス)を設置することで、最終的なVPCのネットワークを構築します。

### パブリックサブネットとプライベートサブネット

セキュリティの観点からは、インターネットと直接通信できるホストは最小限に絞ることが望ましいです。  
これを実現する方法として、ネットワークを「インターネットと直接通信できるサブネット」と「インターネットと直接通信できないサブネット」に分ける方法が一般的に用いられます。

#### ・パブリックサブネット

**インターネットと直接通信できる** サブネットを「 **パブリックサブネット** 」と呼びます。  
パブリックサブネットは外部から(へ)のアクセスが必要なWebサーバやプロキシサーバを設置するという特性上、 [前述のオンプレネットワークにおけるDMZ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%81%AF) と近い役割を果たします。

VPCからインターネットに接続するには [後述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%8D%E3%83%83%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4%E3%81%A8nat%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) インターネットゲートウェイへの接続が必要なので、「 **パブリックサブネット** 」=「 **インターネットゲートウェイにルーティングされるサブネット** 」と言い換えることもできます。

#### ・プライベートサブネット

インターネットと直接通信 **できない** サブネットを「 **プライベートサブネット** 」と呼びます。  
[前述のオンプレネットワークにおける内部ネットワーク](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%81%AF) と近い役割を果たします。

**インターネットとの通信が必要ないインスタンス** をプライベートサブネットに設置する事で、外部からのアクセスを最小限に抑え、 **システム全体のセキュリティを高める** ことができます。

プライベートサブネット内のインスタンスは基本的に同一VPC内のみと通信を行いますが、以下のように踏み台サーバをパブリックサブネットに作成して経由することで、VPC外部からアクセスすることができます。  
(外部 **へ** のアクセスが必要な場合は [後述のNATゲートウェイ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#nat%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) 等を使用します)

[![踏み台サーバ](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F44f6573c-4e31-589d-16c2-7b5acdc9926c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d2b160a330dd09c52ab15f5a52547133)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F44f6573c-4e31-589d-16c2-7b5acdc9926c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d2b160a330dd09c52ab15f5a52547133)

## ルートテーブル

[前述のように](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%AA%E3%83%B3%E3%83%97%E3%83%AC%E3%81%A8vpc%E3%81%AE%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E3%81%AE%E9%81%95%E3%81%84) 、VPCにおいてルーティングによるサブネット外への通信経路制御を実現するのが「 **ルートテーブル** 」です

### ルートテーブルにおける宛先指定方法

VPCのルートテーブルは、 **ロンゲストマッチアルゴリズムに基づき宛先ルート** (VPC画面では「送信先」と表記) **を選択** するという点では、 [オンプレのルーティングテーブル](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%81%AE%E8%A6%8B%E6%96%B9) と共通しています。

一方でオンプレのルーティングテーブルでは「インターフェイス」と「ネクストホップ」に基づきパケットの送信先を定めていたのに対し、VPCのルートテーブルでは **送信先のサービス** を指定します。  
送信先のサービス（以下の実画面における「ターゲット」）には、 [インターネットゲートウェイ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%8D%E3%83%83%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) やEC2のインスタンス、 [Gateway Load Balancer エンドポイント](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9) 等を指定する事ができ、柔軟な経路制御を実現できます。

[![ルートテーブル](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fbdc67b52-0aa9-413a-e259-0434547a0cac.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0dd43757be702f33f8df0610c0563546)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fbdc67b52-0aa9-413a-e259-0434547a0cac.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0dd43757be702f33f8df0610c0563546)

### サブネットごとのルートテーブル指定方法

**ルートテーブルはサブネットごとに作成する事が可能** ですが、この際に重要となる概念が「 **メインルートテーブル** 」と「 **サブネットルートテーブル** 」です

- **メインルートテーブル** ：VPCごとに自動で作成されるルートテーブル(編集も可能)。 **サブネットルートテーブルが設定されていないサブネットに適用** される
- **サブネットルートテーブル** ：ユーザが自分で作成し、サブネットと紐づけるルートテーブル

デフォルト状態で設定したいルートテーブルをメインルートテーブルに記述し、このルールから外れたサブネット独自のルートテーブルを作成したい場合、サブネットルートテーブルを使用するのが良いかと思います（以下参照）

また [前述のよう](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%91%E3%83%96%E3%83%AA%E3%83%83%E3%82%AF%E3%82%B5%E3%83%96%E3%83%8D%E3%83%83%E3%83%88) にインターネットゲートウェイがターゲットに設定されているサブネットが、パブリックサブネットとなります。

  

## VPCと外部ネットワーク・サービスとの接続

VPCとVPC外部のネットワークやサービスとを接続するために、以下のような機能が準備されています  
(よく使われる機能は **太字** にしています)

※ **有料サービスが多い** ので、 [料金にご注意](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%82%B3%E3%82%B9%E3%83%88) ください

| 名称 | VPC内部の接続対象 | 外部の接続対象 | 料金 (リンク先に料金表) |
| --- | --- | --- | --- |
| **インターネットゲートウェイ** | パブリックサブネット | インターネット | [外向き通信量に課金](https://aws.amazon.com/jp/ec2/pricing/on-demand/#Data_Transfer) |
| **NATゲートウェイ** | プライベートサブネット | インターネット(外向き通信のみ) | [利用時間＋通信量に応じ課金](https://aws.amazon.com/jp/vpc/pricing/) |
| Egress Only インターネットゲートウェイ | プライベートサブネット | インターネット(IPv6の外向き通信のみ) |  |
| キャリアゲートウェイ | AWS Wavelength内のサブネット | インターネット(5Gネットワークのみ) | [外向き通信量に課金](https://aws.amazon.com/jp/ec2/pricing/on-demand/#Data_Transfer) |
| **エンドポイント** | VPC全体 | 他のAWSサービス | [インターフェース型のみ有料](https://aws.amazon.com/jp/privatelink/pricing/) (ゲートウェイ型は無料) |
| **エンドポイントサービス** | VPC全体 | 他アカウントのAWSサービス | [後述](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9) |
| **VPCピアリング** | VPC全体 | 他のVPC | [AZをまたぐ場合、通信量に応じ課金](https://aws.amazon.com/jp/ec2/pricing/on-demand/#Data_Transfer_within_the_same_AWS_Region) |
| **トランジットゲートウェイ** | VPC全体 | 他のVPCやオンプレネットワーク | [利用時間＋通信量に応じ課金](https://aws.amazon.com/jp/transit-gateway/pricing/) |

種類が多いですが、基本的には「内部の接続対象」と「外部の接続対象」を決めれば、使用すべきサービスは絞られるかと思います。

以下、詳細を解説します。

### インターネットゲートウェイとNATゲートウェイ

インターネットゲートウェイ、NATゲートウェイは共にVPC内からインターネットに接続するために使用されますが、以下のような差があります

| 名称 | インターネット接続対象のサブネット | IPアドレスの変換方法 | 許可される通信方向 |
| --- | --- | --- | --- |
| インターネットゲートウェイ | パブリックサブネット | [NAT](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#nat-1) | 外向き、内向き |
| NATゲートウェイ | プライベートサブネット | [NAPT](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#napt) | 外向きのみ |

#### ・インターネットゲートウェイ

インターネットゲートウェイは **パブリックサブネットを作成する際に必ず必要となる** サービスであり、インターネットとの外向き、内向き **双方向** の通信を許可します。

インターネットゲートウェイではグローバルIPアドレスとプライベートIPアドレスが1対1で変換されるため、 **NAT** としての機能を果たします。

#### ・NATゲートウェイ

プライベートサブネットからは通常インターネットにアクセスできません( [後述のエンドポイント経由のyumコマンド](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E7%B5%8C%E7%94%B1%E3%81%A7%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%82%B5%E3%83%96%E3%83%8D%E3%83%83%E3%83%88%E3%81%8B%E3%82%89yum%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E5%AE%9F%E8%A1%8C) を除く)が、セキュリティパッチ更新等のためにインターネットにアクセスしたい場面も度々生じます。このような用途のために **プライベートサブネット** から **外向きの通信のみを許可** する(内向きは許可しない)サービスが、 **NATゲートウェイ** です

NATゲートウェイはプライベートサブネットのインターネット接続を実現するサービスですが、 **NATゲートウェイ自身はパブリックサブネット上に設置** されます。

なお名前が紛らわしいですが、NATゲートウェイは [前述した狭義のNAT](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#nat-1) ではなく、ポートを含めて1対多でグローバルIPとプライベートIPを変換する [NAPT](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#napt) としての機能を果たします。  
グローバルIPとプライベートIPを1対1変換する狭義のNATの役割を果たすのは「インターネットゲートウェイ」ですので、ご注意ください  
詳細は以下の記事が分かりやすいです。

また、 **NATゲートウェイは比較的料金お高めのサービス** のため、多くの場合 [後述のエンドポイント](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88-vpc%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88) 等の **他サービスを使用した方がコストを節減できる場合が多い** です。NATゲートウェイの使用頻度を減らしてコストダウンする方法については、以下の記事が分かりやすいです。

上記記事に記載されているように、構成によっては「NATインスタンス」を使った方がコストを節減できることもあります。NATインスタンスはNATゲートウェイと同様の機能をインスタンスとして実現する古いサービスですが、ユーザ側の管理の手間が増えるため、大きな価格差がないのであればNATゲートウェイを使用することが推奨されています。

### Egress Only インターネットゲートウェイ

IPv6専用のインターネットゲートウェイで、 **外向きの通信のみを許可** する(内向きは許可しない)という意味で、IPv4におけるNATゲートウェイと同様の役割を果たします("Egress"とは「出力」を表します)

詳細は以下の記事をご参照ください

### キャリアゲートウェイ

[**AWS Wavelength**](https://aws.amazon.com/jp/wavelength/) と呼ばれる、 **5Gモバイルネットワーク専用のAZ** と5Gネットワーク網との間に設置されるゲートウェイです。

こちらも新しいサービスのため情報が少ないですが、以下の方が記事にされています。

なお、この機能は大阪リージョンでは使用できません。

### エンドポイント (VPCエンドポイント)

エンドポイントは、VPC内のインスタンスと **VPC外のAWSサービス** とを **インターネットを経由しない接続** (プライベート接続)で通信できるようにする機能です。

VPCと他のAWSサービスとの接続はインターネットゲートウェイやNATゲートウェイ経由でも実現できますが、 [**コスト面でエンドポイントを使用した方がメリットが大きい**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%82%B3%E3%82%B9%E3%83%88) です。

エンドポイントは大きく以下の2種類に分けられます

- **インターフェイスエンドポイント** ： **AWSサービスにVPC内のIPアドレスが割り振られる** (仮想NICとして機能する)方式。見かけ上はVPC内での通信で完結しているように見えるため、 **設定がシンプルで管理しやすい** が、アクセスに **料金が発生** する (下図の `Endpoint network interface` が相当)
[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fdocs.aws.amazon.com%2Fja_jp%2Fvpc%2Flatest%2Fprivatelink%2Fimages%2Fvpc-endpoint-kinesis-private-dns-diagram.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f332b561babf80ec912ad4eb2e001e46)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fdocs.aws.amazon.com%2Fja_jp%2Fvpc%2Flatest%2Fprivatelink%2Fimages%2Fvpc-endpoint-kinesis-private-dns-diagram.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f332b561babf80ec912ad4eb2e001e46)
- **ゲートウェイエンドポイント** ： **グローバルIPを持ったAWSサービス** へのルーティング設定が追加される方式。VPC外の機器という扱いとなるため、ACL等の **設定がやや複雑** となるが、アクセスは **無料** 。S3とDynamoDBのみ使用可能 (下図の `VPC endpoint` が相当)
[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fdocs.aws.amazon.com%2Fja_jp%2Fvpc%2Flatest%2Fprivatelink%2Fimages%2Fvpc-endpoint-s3-diagram.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=25ed54f74fd3e39832e685f6f897830b)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fdocs.aws.amazon.com%2Fja_jp%2Fvpc%2Flatest%2Fprivatelink%2Fimages%2Fvpc-endpoint-s3-diagram.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=25ed54f74fd3e39832e685f6f897830b)

図を見ると分かりますが、 **接続先のAWSサービス** は **インターフェイスエンドポイントではVPC内のホスト** (サブネット内にIPアドレスが存在する)、 **ゲートウェイエンドポイントではVPC外のホスト** (VPC外のグローバルIPアドレスが割り振られ、ルーター経由でルーティングされる)として認識されていることが分かります  
詳細は以下をご参照ください

なお、2021年以降のAWSサービス間の通信は基本的にエンドポイントを利用しなくともインターネットを経由しないようです。  
よって、よくテック記事等で見かける「エンドポイントを利用しないと通信がインターネットを経由するが、利用するとインターネットを経由せずセキュアになる」というメリットは成立しなくなっていますが、以下のようにセキュリティ面、コスト面で多数のメリットが存在するため、 **VPCと他のAWSサービスとの接続時** には、 **エンドポイントをインターネットゲートウェイやNATゲートウェイよりも優先的に採用** して良いかと思います。

#### ※ゲートウェイエンドポイント経由でプライベートサブネットからyumコマンド実行

通常、プライベートサブネットからインターネットへアクセスするにはNATゲートウェイが必要となります。  
これはyumコマンドで各種パッケージをアップデートする際も同様です。

一方で、 **OSにAmazon Linuxを使用している場合** のみ、 **NATゲートウェイなしでもS3用のゲートウェイエンドポイント経由でyumコマンドを実行** する事ができます。

これにより、コストが増大しがちなNATゲートウェイなしでパッケージのアップデートを実現できるので、 **コスト削減** にぜひご活用下さい

### エンドポイントサービス

エンドポイントサービスは、 **他のAWSアカウント** に **VPC内のサービスへのインターネットを経由しないアクセス** を提供する機能です。

エンドポイントサービスは大きく以下の2種類に分けられます

| 名称 | 概要 | 料金 |
| --- | --- | --- |
| **インターフェイスVPCエンドポイント** | Network Load Balancer(NLB)経由でVPC内のサービスへアクセスする。最近まで唯一の選択肢だった | [インターフェイスエンドポイントのコスト](https://aws.amazon.com/jp/privatelink/pricing/) ＋ [NLBのコスト](https://aws.amazon.com/jp/elasticloadbalancing/pricing/) |
| **Gateway Load Balancerエンドポイント** | Gateway Load Balancer(GWLB)経由でVPC内のサービスへアクセスする。2020年に追加された新しいサービスで、スケーリングやサードパーティのセキュリティ製品との連携に [優れるとのこと](https://dev.classmethod.jp/articles/choosing-the-right-load-balancer-for-serverless-applications-net301-in-reinvent2020/) | [Gateway Load Balancerエンドポイント](https://aws.amazon.com/jp/privatelink/pricing/) ＋ [GWLBのコスト](https://aws.amazon.com/jp/elasticloadbalancing/pricing/) |

なお、「 **エンドポイントサービス** 」は [前述](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88-vpc%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88) の「 **エンドポイント** 」と **AWS内のサービス間でインターネットを経由しないセキュアな通信を提供** するという意味で共通しており、 **まとめて"PrivateLink"と呼ばれる** 事もあります。

### VPCピアリング

VPCピアリングとは、 **VPC同士を接続** し、お互いに通信できるようにするための機能です。  
VPC同士を接続するケーブルの役割を果たします。

VPCピアリングではルーティングテーブルに相手のVPCへのルーティングが追加されるため、オンプレネットワークにおけるルーターのようにふるまいます。

[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fdocs.aws.amazon.com%2Fja_jp%2Fvpc%2Flatest%2Fpeering%2Fimages%2Fpeering-intro-diagram.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e14532a158ad83ca4ec3633556b03525)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fdocs.aws.amazon.com%2Fja_jp%2Fvpc%2Flatest%2Fpeering%2Fimages%2Fpeering-intro-diagram.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e14532a158ad83ca4ec3633556b03525) ※画像はAWS公式より

接続対象のVPCは **異なるアカウントのものでもよく** 、 **異なるリージョンにあってもよい** ですが、異なるリージョンにある場合IPv6での通信ができないことにご注意ください。

なお、CIDRブロックの重複が許されない、ピアリングを経由したインターネットやVPNへの接続ができない等、ピアリング経由の通信にはいくつか制約があります。詳細は以下の記事が分かりやすいです。

### トランジットゲートウェイ

トランジットゲートウェイは、VPCピアリングと同様に **VPC同士を接続** できるサービスですが、VPCピアリングが2個のVPCを接続するケーブルの役割を果たすのに対して、トランジットゲートウェイは **3個以上のVPCを放射状に接続** する事ができます。

[![VPCピアリングとトランジットゲートウェイ](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fe0126027-3133-1f24-eab6-ce02ec28b677.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6be2871e53a3318774acc5ffad728ad8)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fe0126027-3133-1f24-eab6-ce02ec28b677.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6be2871e53a3318774acc5ffad728ad8)

**多数のVPC同士を接続** したい際に構成がシンプルになるのみならず、オンプレネットワークとの **VPN接続** ( [後述](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpn-%E4%BB%AE%E6%83%B3%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF) )を追加することもできます。

但し、 **VPCピアリングよりも若干通信料金がお高め** のため、どちらを選択するかはコストと利便性のバランスを見て判断してください。

トランジットゲートウェイにも異なるリージョン間のVPCを直接接続できない等の制約があります。詳細は以下の記事が参考になります。

  

## VPN (仮想プライベートネットワーク)

仮想プライベートネットワークは、 **VPCとオンプレネットワーク間に仮想的なプライベートネットワークを作成** するサービスです。

VPNの通信路は実際にはインターネット内に作成されるのですが、 [**トンネリング**](https://ja.wikipedia.org/wiki/%E3%83%88%E3%83%B3%E3%83%8D%E3%83%AA%E3%83%B3%E3%82%B0) と呼ばれる技術を用いる事によって、遠く離れた2点 (AWSの場合、VPCとオンプレネットワークそれぞれのゲートウェイ)をあたかも同一点であるかのように扱えます。  
これにより **セキュアな通信路を確保** できるため、社内ネットワーク等によく利用されています。

AWSでは、以下の2種類のVPNを使用できます。

| 名称 | オンプレ側のVPN接続地点 | トンネリングプロトコル | 料金 (リンク先に料金表) |
| --- | --- | --- | --- |
| **Site-to-Site VPN** | オンプレネットワーク上の **ルーター**   (カスタマーゲートウェイ) | IPsec | [利用時間※＋通信量に応じ課金](https://docs.aws.amazon.com/ja_jp/vpn/latest/s2svpn/VPC_VPN.html#pricing) |
| **クライアントVPN** | **クライアントPC** | SSL-VPN | [利用時間に応じ課金](https://docs.aws.amazon.com/ja_jp/vpn/latest/clientvpn-admin/what-is.html#what-is-pricing) |

※Acceleratedサイト間VPNで高速化すると時間料金に追加料金が上乗せされます

また、VPN以外にもVPCとオンプレを専用線で繋ぐ [Direct Connect](https://aws.amazon.com/jp/directconnect/) というサービスも存在します

### Site-to-Site VPN (サイト間のVPN接続)

Site-to-Site VPNは、 **VPC** の [仮想プライベートゲートウェイ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E4%BB%AE%E6%83%B3%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) と、 **オンプレネットワーク上のルーター** ( [カスタマーゲートウェイ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%83%BC%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) )との間にトンネリング通信路を作成します。

**・Site-to-Site VPNの概要図**  
[![Site-to-Site VPN](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fa41bead3-dfa6-a7b1-fd82-983f2427a301.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=22f092414dce453555e3a25c13e077ca)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2Fa41bead3-dfa6-a7b1-fd82-983f2427a301.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=22f092414dce453555e3a25c13e077ca)

上図のように、VPNで接続された **プライベートネットワーク内はインターネットから直接アクセスできない** ため、 **オンプレネットワークとVPC間にセキュアな通信路** を確保できます。

VPNには [いくつかの種類](https://www.yamanjo.net/knowledge/internet/internet_36.html) がありますが、Site-to-Site VPNでは [IPsec](https://sc.seeeko.com/entry/ipsec) と呼ばれる方式が用いられています。

以下、各種ゲートウェイとこれらを繋ぐトンネリング (Site-to-Site VPN)について解説します

#### ・仮想プライベートゲートウェイ

トンネリングの **AWS側の出口** となるゲートウェイです。

#### ・カスタマーゲートウェイ

トンネリングの **オンプレ側の出口** となるゲートウェイです。

利用にはゲートウェイとして設定するオンプレミス環境のルーターのIPアドレスや [AS番号](https://www.nic.ad.jp/ja/ip/asnumber.html) 、 [認証のための証明書](https://aws.amazon.com/jp/vpn/faqs/) が必要となります。

#### ・サイト間のVPN接続 (Site-to-Site VPN)

上記のゲートウェイ間を接続するトンネリング通信路を作成します。

通常のVPNに加えて、 **追加料金** を払って「 **Acceleratedサイト間VPN** 」を有効化すると、 **高速** なVPN接続を実現できます。

各種ゲートウェイとSite-to-Site VPNの接続パターンは、以下の記事が参考になります。  
( [前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%88%E3%83%A9%E3%83%B3%E3%82%B8%E3%83%83%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) トランジットゲートウェイと組み合わせることもできます)

### クライアントVPN

Site-to-Site VPNではVPCとオンプレネットワーク上のルータを接続していました。  
一方でクライアントVPNでは、VPC上に設置した [クライアントVPNエンドポイント](https://docs.aws.amazon.com/ja_jp/vpn/latest/clientvpn-admin/cvpn-working-endpoints.html) と **クライアントPC** 間に **直接トンネリング通信路** を作成することができます。

ルータを介する必要がないので、 **ノートPC等でVPNを使用したいとき** に非常に便利です。

[![クライアントVPN](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F2c361779-164e-495c-a356-5f1c59deadc2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7af25a38603988cf5cd601804e7c2936)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F2c361779-164e-495c-a356-5f1c59deadc2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7af25a38603988cf5cd601804e7c2936)

VPN接続はクライアントPCに [OpenVPN](https://www.openvpn.jp/) というオープンソースのVPNソフトウェアをインストールする事で実現し、SSL-VPNと呼ばれる方式 (HTTPS通信で使われるSSL/TLSと同様の暗号化方式)が用いられています。

またVPC側には [クライアントVPNエンドポイント](https://docs.aws.amazon.com/ja_jp/vpn/latest/clientvpn-admin/cvpn-working-endpoints.html) を設置する必要があります。  
設定方法は以下の記事が分かりやすいです。

  

## VPCのセキュリティ機能

VPCにはセキュリティを高めるための [**ファイアウォール**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A8acl) **に相当する機能** として、 **以下の3サービス** が準備されています。

| 名称 | 外部側フィルタ対象 | フィルタ方式 | 戻り通信の扱い | 内部側フィルタ対象 |
| --- | --- | --- | --- | --- |
| **ネットワークACL** | 外部側IPアドレス + ポート番号 + [プロトコル](http://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) | ホワイトリスト + ブラックリスト | ステートレス | サブネットごと |
| **セキュリティグループ** | 外部側IPアドレス ( [セキュリティグループID](https://www.sunnycloud.jp/column/20210716-01/), [プレフィックスリスト](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%9E%E3%83%8D%E3%83%BC%E3%82%B8%E3%83%89%E3%83%97%E3%83%AC%E3%83%95%E3%82%A3%E3%83%83%E3%82%AF%E3%82%B9%E3%83%AA%E3%82%B9%E3%83%88) も指定可) + ポート番号 + [プロトコル](http://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) | **ホワイトリスト** | **ステートフル** | **インスタンスごと** (セキュリティグループIDでグルーピング) |
| **ネットワークファイアウォール** | 外部側IPアドレス (ドメインリストも指定可) + ポート番号 + [プロトコル](http://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) | ホワイトリスト + ブラックリスト | ステートフル + ステートレス | サブネットごと (ドメインリストも指定可) |

#### ※「プロトコル」とは？

フィルタ対象として [前述のIPアドレスとポート番号](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A8acl) に加え、「 **プロトコル** 」と呼ばれる要素が加わっていますが、何を表すのでしょうか？

この「プロトコル」は、 [IANAが定義するトランスポート層のプロトコル番号](http://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) を指しており、UDPとTCP等、IPアドレスとポート番号だけでは見分けられれないプロトコルを特定するために使用されます。  
UDPやTCP等の **トランスポート層プロトコル** と、ICMP等の **ネットワーク層プロトコルが混在** しているため、混乱しやすいのでご注意ください。

とはいえ、実際に選択する際には該当するアプリケーション層プロトコルの名称が併記されているので、それほど迷う事はないと思います。

### どれを使う？

前者2つの違いは以下の記事で分かりやすくまとめられております

AWS公式では触れられていませんが、以下のような多くのサードパーティーの記事では、 **ネットワークACLよりもセキュリティグループを優先して使用** する事が **推奨** されています。

主な理由として、 [ステートレス](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%B9%E3%83%86%E3%83%BC%E3%83%88%E3%83%AC%E3%82%B9) かつ [ホワイトリストとブラックリスト](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A8acl) 両方を指定可能な **ネットワークACLは設定が煩雑** で、 [ステートフル](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%B9%E3%83%86%E3%83%BC%E3%83%88%E3%83%95%E3%83%AB) かつホワイトリストのみで指定する **セキュリティグループは設定がシンプル** であり、 **ミス等を防ぎやすい** 事が挙げられます (他にも、サブネット単位でしかルール設定できないネットワークACLと比べ、セキュリティグループはインスタンス単位でより細かいアクセス制御ができることも挙げられます)

以下で各機能について詳説します。

### ネットワークACL

ネットワークACLは、 **サブネットごとにIPアドレスやポート番号(通信プロトコル)に基づきアクセス制限を設定する** 機能で、その名の通り [ACL](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A8acl) を使用します。

前述の「 [ファイアウォールとACL](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB%E3%81%A8acl) 」の定義と照らし合わせると、ネットワークACLは

- **ホワイトリスト** (許可)と **ブラックリスト** (拒否) **両方** を設定可
- **行きと戻りを別々に評価するステートレス** 方式  
	の評価方法となります。

[前述のように](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%B9%E3%83%86%E3%83%BC%E3%83%88%E3%83%95%E3%83%AB) 、多くのファイアウォールでは設定をシンプルにするために「ステートフル」なACLを採用している事が多いですが、 **ネットワークACLは** 「ステートレス」のため、 **設定が煩雑** となりがちです。

そのため、多くの記事では **ネットワークACLよりも** ステートフルな [**セキュリティグループ (後述)**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%82%B0%E3%83%AB%E3%83%BC%E3%83%97-1) を **優先して使用** することを推奨しています。

セキュリティグループの使用が推奨される理由は他にもいくつかあり、以下の記事で分かりやすく解説されております。

なお、VPC作成時にはデフォルトのネットワークACLが作成され、VPC内の全サブネットに必ず適用されます。  
デフォルトのネットワークACLは全ての通信を許可する設定となっている(＝一切のアクセス制限を実施しない)ため、ここから追加でセキュリティグループやネットワークACLを編集・作成することで、アクセス制限を適用する事ができます。

### セキュリティグループ

セキュリティグループも、ネットワークACLと同様に **ルールに基づくVPCへのアクセス制限** を実現する機能ですが、 **インスタンス単位** 、あるいはインスタンスのグループ単位でアクセス制御可能 (ネットワークACLはサブネット単位)であることが特徴です。

ネットワークACLはフィルタリングのルールにサブネット(CIDR)を指定しますが、セキュリティグループはCIDRに加え [**セキュリティグループID**](https://www.sunnycloud.jp/column/20210716-01/) 、 [**マネージドプレフィックスリスト**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%9E%E3%83%8D%E3%83%BC%E3%82%B8%E3%83%89%E3%83%97%E3%83%AC%E3%83%95%E3%82%A3%E3%83%83%E3%82%AF%E3%82%B9%E3%83%AA%E3%82%B9%E3%83%88) も指定する事ができるため、より **柔軟で細かいアクセス制御** が可能となります。

他にも

- **ホワイトリスト** (ネットワークACLはブラックリスト＋ホワイトリスト)
- **ステートフル** (ネットワークACLはステートレス)

という特徴を持ち、 **ネットワークACLと比べてシンプルでミスを防止しやすい** 設定が可能と言えます。

なお、セキュリティグループに設定可能なインスタンスはEC2に限らず、RDS(データベース)やELB(ロードバランサ)  
等も設定できます。

また 詳細は別記事(作成中)で紹介しますが 、 **同一セキュリティグループ内の通信も明示的に許可しないとアクセス不可** である事にご注意ください (内部間のアクセスは遮断しないオンプレのファイアウォールと挙動が異なる)

### ネットワークファイアウォール

セキュリティグループはホワイトリスト形式のため「特定のターゲットをブロックしたい」といった用途で使用する事は出来ません。また、ブラックリスト形式のネットワークACLはステートレスなため、 [前述のように](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AFacl) 設定が複雑となりミスを引き起こしやすいというデメリットが存在します。

そこで、両者の欠点を補えるよう2020年に登場したサービスが、 **ネットワークファイアウォール** です。  
ネットワークファイアウォールは **ホワイトリスト・ブラックリスト両者** 、かつ **ステートレス・ステートフル両者** を設定可能で、 **柔軟なアクセス制御** を実現できます。

また、ネットワークACLやセキュリティグループはVPC上に「ルール」を設定するだけで動作しましたが、ネットワークファイアウォールは以下のようにVPCのサブネット上に「 **ファイアウォールエンドポイント** 」という **オブジェクトを設置する必要** があります  
（ファイアウォールエンドポイントはルートテーブル上では [Gateway Load Balancer エンドポイント](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9) として認識されます。またエンドポイントはAZごとに設置する必要があります）

[![ネットワークファイアウォール](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F950e77e5-e9d3-68e1-3866-ff1c9a79ce1d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=35c0c9cbfcc8ec5c843ae55d4a867827)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F950e77e5-e9d3-68e1-3866-ff1c9a79ce1d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=35c0c9cbfcc8ec5c843ae55d4a867827)

注意点として、ネットワークファイアウォール使用時は **ルートテーブルを編集** して、 **インターネットとの通信が必ずファイアウォールエンドポイントを経由するよう設定** する必要があります。  
例えばWebアプリにおけるネットワークファイアウォールの設置方法や、WAF・サードパーティのファイアウォールとの使い分けに関しては、以下の公式ドキュメントで詳細に解説されています。

ネットワークファイアウォールにより通信の流れを上記のような図で可視化する事ができるため、オンプレのネットワークに慣れている人には直感的に理解しやすい構成となります。一方でルートテーブル等の **設定すべき項目が複雑** となってしまう事や、 **料金が高め** であること(かつAZごとに必要なため、冗長化するとさらに高額となる)がデメリットとなります。  
よって比較的上級者向けのサービスと言え、 **初心者はなるべくセキュリティグループを使用する事が望ましい** でしょう。

  

## IPアドレス管理用サービス

**デフォルト設定** では、サブネット内部に作成したインスタンスの **IPアドレスは成り行きで動的に作成** されます。

**成り行きでは管理が難しい** ため、 **IPアドレスを固定したり、複数のIPアドレスをグルーピングする** ことで、より制御された管理を実現するために以下のようなサービスが準備されています。

| 名称 | 概要 |
| --- | --- |
| **Elastic IP** | インスタンスに固定のグローバルIPアドレスを割り振る |
| **マネージドプレフィックスリスト** | 複数のIPアドレスをグルーピングし、セキュリティグループやルーティングの設定を一括管理する |

### Elastic IP

通常、VPC内部のインスタンスのグローバルIPアドレスは、 [DHCP機能](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#dhcp) により動的に付与されるため、インスタンスを停止 ⇨ 再起動するたびにIPアドレスが変わってしまいます。  
このIPアドレスの変化を防ぐために **固定のグローバルIPアドレス** を割り振る機能が、Elastic IP (EIP)です。  
Elastic IPにより、インスタンスを再起動しても同一のIPアドレスでアクセスできるようになります。

Elastic IPは **稼働中のEC2インスタンスに付与した場合は無料** ですが、 **休止中のEC2インスタンスに使用する場合やインスタンスと紐づいていない場合** は$0.005/hの **料金が掛かる** のでご注意ください。

Elastic IPは、EC2インスタンス以外にもNATゲートウェイ、 [Network Load Balancer(NLB)](https://dev.classmethod.jp/articles/elb-network-load-balancer-static-ip-adress/) でも用いる事ができ、この場合1つのElastic IPアドレスを複数のインスタンスやサービスで共有する事ができます。

### マネージドプレフィックスリスト

マネージドプレフィックスリストは、 **複数のプレフィックス** (CIDRブロック、サブネット)を **グルーピング** するための機能です。

マネージドプレフィックスリストを利用することで、 **セキュリティグループやルートテーブルの設定を一括管理できる** ため、管理の手間を減らす事ができます。

マネージドプレフィックスリストには、 **次の2つのタイプ** があります。

- **カスタマー管理プレフィクスリスト** ：ユーザ側で独自に作成したプレフィックスリスト
- **AWSマネージドプレフィクスリスト** ：AWS側で予め準備されているプレフィックスリスト。カスタマー管理プレフィックスリストと同様にセキュリティグループやルートテーブルを設定できるが、ユーザ側でプレフィックスリスト自体を変更、削除することはできない

詳細は以下の記事が参考になります。

また2022/5現在、AWSマネージドプレフィクスリストには以下の3種類 (CloudFront, S3, DynamoDB用)が準備されています。

[![AWSマネージドプレフィクスリスト](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F50d29a12-5d5a-99f5-1fa7-a060d0658a08.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3447983d9fb55008db619ae3d8446b33)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F50d29a12-5d5a-99f5-1fa7-a060d0658a08.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3447983d9fb55008db619ae3d8446b33)

このうち **S3, DynamoDB用** は [前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88-vpc%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88) **ゲートウェイエンドポイント使用時にルートテーブルの送信先に設定** するため、VPC内からS3, DynamoDBにアクセスする際は重要な役割を果たします (S3での設定法に関しては 別記事で紹介(記事作成中) します)

  

## VPCとログ

VPCには、内部のENI (EC2インスタンス, ELBなどに紐づけられたネットワークインターフェイス)に出入りする **IPトラフィックを記録** する **フローログ** と呼ばれる機能が存在します

[公式ドキュメント](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/flow-logs.html) では、フローログのユースケースとして以下が挙げられています

- 制限が厳しすぎるセキュリティグループルールを診断する (想定外の通信拒否の有無を確認できる)
- インスタンスに到達するトラフィックをモニタリングする
- ネットワークインターフェイスに出入りするトラフィックの方向を特定する

以下で、実際に記録される内容と保存場所について解説します

### フローログの構造

フローログは、以下のような構造となっています。　　

Copied!
```text
2 123456789010 eni-1235b8ca123456789 172.31.16.139 172.31.16.21 20641 22 6 20 4249 1418530010 1418530070 ACCEPT OK
```

上記だけでは分かりづらいですが、以下のようなフィールドから構成されています( [公式ドキュメントでの解説](https://docs.aws.amazon.com/ja_jp/vpc/latest/userguide/flow-logs.html#flow-log-records) )

| フィールド名 | 上例での値 | 内容 |
| --- | --- | --- |
| version | 2 | フローログのバージョン |
| account-id | 123456789010 | VPCが所属するAWSアカウントID |
| interface-id | eni-1235b8ca123456789 | トラフィックが記録されるENIのID (送受信元のインスタンスやELB等と紐づく) |
| srcaddr | 172.31.16.139 | 送信元のIPアドレス |
| dstaddr | 172.31.16.21 | 受信先のIPアドレス |
| srcport | 20641 | 送信元ポート |
| dstport | 22 | 受信先ポート |
| protocol | 6 | トラフィックの [IANAプロトコル番号](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) |
| packets | 20 | フロー中に転送されたパケットの数 |
| bytes | 4249 | フロー中に転送されたバイト数 |
| start | 1418530010 | フローの最初のパケットが受信された時間 ( [UNIX秒](https://ja.wikipedia.org/wiki/UNIX%E6%99%82%E9%96%93) ) |
| end | 1418530070 | フローの最後のパケットが受信された時間 ( [UNIX秒](https://ja.wikipedia.org/wiki/UNIX%E6%99%82%E9%96%93) ) |
| action | ACCEPT | トラフィックの許可状況   (ACCEPT: 承認、   REJECT: セキュリティグループやネットワークACL等により拒否) |
| log-status | OK | ログのステータス   (OK: データは選択された送信先に正常に記録、   NODATA: 集約間隔内に対象ENIへのトラフィックなし、   SKIPDATA: 集約間隔内に一部のフローログレコードがスキップ (内部的なキャパシティー制限、または内部エラーが原因の可能性あり)) |

例えば `action` フィールドから想定外のREJECTとなっているトラフィックがないかを確認することで、前述の「制限が厳しすぎるセキュリティグループルールの診断」を実施する事ができます。

見ての通り生のログを目視で確認するのは大変ですが、他の多くのログと同様に [CloudWatch Logs](https://aws.amazon.com/jp/premiumsupport/knowledge-center/cloudwatch-vpc-flow-logs/) で要約や各種検知を実施したり、 [Amazon Athena](https://docs.aws.amazon.com/ja_jp/athena/latest/ug/vpc-flow-logs.html) を利用して分析すると効率よく活用できるかと思います

### フローログの保存場所

フローログの保存場所は、以下から選択する事ができます。

- **CloudWatch Logs**
- **S3**

[こちらのサイト](https://dev.classmethod.jp/articles/vpc-flow-logs-athena/) をみる限り、2つの保存場所は以下のように使い分けるとよさそうです。

| フローログの保存場所 | 用途 |
| --- | --- |
| **CloudWatch Logs** | ネットワークトラフィックの特定のアクセス傾向を検知してアラームを発する場合 |
| **S3** | フローログを蓄積してAthenaで分析する場合 |

具体的なフローログの取得方法は、以下の記事で解説します

記事作成中

### フローログの料金

フローログの料金は、以下のページから参照する事ができます。

保存場所にCloudWatch Logsを使用する場合は「例4. CloudWatch Logsに配信されたVPCフローログのモニタリング」を、  
S3を使用する場合は「例3. S3に配信されたVPCフローログのモニタリング」を参照下さい

  
  

## VPCとコスト

VPCの **料金** については、以下のサイトが大変分かりやすいので、こちらをご参照ください。

要点としては、以下に特に気をつける必要があるかと思います。

- [**NATゲートウェイ**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#nat%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) に **コストが掛かりがち** なので、むやみに大量使用しない
- NATゲートウェイのコスト削減のため、 **S3やDynamoDBアクセス時** は [**エンドポイント**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88-vpc%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88) を使用すると良い
- AWS外部からの **受信は無料** だが、 **外部への送信はデータ転送量がかかる**
- AWS内部でも、 **リージョンを跨ぐとデータ転送量が増大** する

また、フローログの料金に関しては前述の [「フローログの料金」](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%95%E3%83%AD%E3%83%BC%E3%83%AD%E3%82%B0%E3%81%AE%E6%96%99%E9%87%91) を参照ください

  
  

## VPCコンソール画面の見方

AWSコンソールにログインしてVPCのサービスに入ると、以下のような画面が表示されます。  
[![VPCコンソール画面](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F97613118-9687-31d5-4d23-89fda4c722e9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c1f974d25f1563ba0cbc0bae7ba42cef)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F97613118-9687-31d5-4d23-89fda4c722e9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c1f974d25f1563ba0cbc0bae7ba42cef)

「VPCダッシュボード」は、現在のアカウントが保持しているVPCリソースが一覧表示されています。

「左側のタブ」（ナビゲーションペイン）からはVPC **各リソースの設定画面** に飛べるので、それぞれ簡単に解説します

## EC2 Global View

全リージョンのVPCリソースおよびEC2インスタンスの数が一覧表示されます。  
**他のリージョンの情報** も含め簡易的に確認したい場合に便利です

## VIRTUAL PRIVATE CLOUD

### VPC

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%81%9D%E3%81%AE%E4%B8%8A%E9%99%90%E5%80%8B%E6%95%B0) VPCの作成、一覧表示、設定を行います。

### サブネット

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%82%B5%E3%83%96%E3%83%8D%E3%83%83%E3%83%88) サブネットの作成、一覧表示、設定を行います。

### ルートテーブル

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%AB%E3%83%BC%E3%83%88%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB) ルートテーブルの作成、一覧表示、設定を行います。

### インターネットゲートウェイ

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%8D%E3%83%83%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4%E3%81%A8nat%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) インターネットゲートウェイの作成、一覧表示、設定を行います。

### Egress Only インターネットゲートウェイ

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#egress-only-%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%8D%E3%83%83%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) Egress Only インターネットゲートウェイの作成、一覧表示、設定を行います。

### キャリアゲートウェイ

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%AD%E3%83%A3%E3%83%AA%E3%82%A2%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) キャリアゲートウェイの作成、一覧表示、設定を行います。

### DHCPオプションセット

DHCPと名前が付いていますが、 [DHCPサーバの主機能として解説した](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#dhcp) 動的なIPアドレスの振り分けでなく、 **VPC内部のインスタンスから使用するDNSサーバやNTCサーバを指定** するために使用されます。  
( [Elastic IPの項で解説したように](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#elastic-ip) VPC内部のIPアドレスはデフォルトで動的に振り分けられます)

**内部のEC2インスタンス等からDNSサーバやNTCサーバを使用したい場合** 、一括で指定ができて便利です。

詳細は以下の記事が分かりやすいです。

### Elastic IP

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#elastic-ip) Elastic IPの作成、一覧表示、設定を行います。

### マネージドプレフィックスリスト

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%9E%E3%83%8D%E3%83%BC%E3%82%B8%E3%83%89%E3%83%97%E3%83%AC%E3%83%95%E3%82%A3%E3%83%83%E3%82%AF%E3%82%B9%E3%83%AA%E3%82%B9%E3%83%88) マネージドプレフィックスリストの作成、一覧表示、設定を行います。

### エンドポイント

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88-vpc%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88) エンドポイントの作成、一覧表示、設定を行います。

### エンドポイントのサービス

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%A8%E3%83%B3%E3%83%89%E3%83%9D%E3%82%A4%E3%83%B3%E3%83%88%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9) エンドポイントサービスの作成、一覧表示、設定を行います。

### NATゲートウェイ

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#nat%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) NATゲートウェイの作成、一覧表示、設定を行います。

### ピアリング接続

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%83%94%E3%82%A2%E3%83%AA%E3%83%B3%E3%82%B0) VPCピアリングの作成、一覧表示、設定を行います。

## セキュリティ

### ネットワーク ACL

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AFacl) ネットワークACLの作成、一覧表示、設定を行います。

### セキュリティグループ

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%82%B0%E3%83%AB%E3%83%BC%E3%83%97) セキュリティグループの作成、一覧表示、設定を行います。

### Reachability Analyzer

VPC内の任意の2つのサービス(リソース)間において、お互いが通信できるかを分析するツールです。  
VPCやサービス作成後の動作確認、トラブル時の疎通確認等に使用できます ( [**有料**](https://aws.amazon.com/jp/vpc/pricing/) サービスなのでご注意ください)

詳細は以下の記事が分かりやすいです

※2022/6現在、大阪リージョンでは使用できません

## NETWORK ANALYSIS

### Network Access Analyzer

ネットワークがネットワークアクセス要件を満たしているかを分析するツールです ( [**有料**](https://aws.amazon.com/jp/vpc/pricing/) サービスなのでご注意ください)

「ネットワークアクセス要件」だけでは何のことだか分かりませんが、主に [**VPCと外部ネットワーク・サービスとの接続**](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E5%A4%96%E9%83%A8%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%A8%E3%81%AE%E6%8E%A5%E7%B6%9A) で紹介した **外部との接続サービスへの経路を可視化** し、想定外の経路=セキュリティリスクが存在しないかを分析する事ができます。

使用法等は以下の記事が分かりやすいです

※2022/6現在、大阪リージョンでは使用できません

## DNS Firewall

通常のファイアウォール (前述の [セキュリティグループ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%82%B0%E3%83%AB%E3%83%BC%E3%83%97) や [ネットワークACL](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AFacl),[ネットワークファイアウォール](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB) )はIPアドレスとポート番号に基づき通信を制限しますが、 **DNS Firewall** は **ドメイン名** (ホスト名)の **名前解決に制限を加える** 機能となります。

これにより、サイバー攻撃( [DNSキャッシュ汚染](https://sc.seeeko.com/entry/dns_security#1DNS%E3%82%AD%E3%83%A3%E3%83%83%E3%82%B7%E3%83%A5%E3%83%9D%E3%82%A4%E3%82%BA%E3%83%8B%E3%83%B3%E3%82%B0),[マルウェア感染](https://sc.seeeko.com/entry/malware#1%E3%82%A6%E3%82%A4%E3%83%AB%E3%82%B9%E3%81%A8%E3%83%9E%E3%83%AB%E3%82%A6%E3%82%A7%E3%82%A2) 等)の第2段階として行われる **不正なサーバ** (C&Cサーバ) **との通信を防ぐ** 事ができます

厳密にはRoute53の機能となるため詳細解説は割愛しますが、以下の記事が参考になります

## ネットワークファイアウォール

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB) ネットワークファイアウォールの作成、一覧表示、設定を行います。

## 仮想プライベートネットワーク (VPN)

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpn-%E4%BB%AE%E6%83%B3%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF) VPNを構成する [カスタマーゲートウェイ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%83%BC%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) 、 [仮想プライベートゲートウェイ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E4%BB%AE%E6%83%B3%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) 、 [Site-to-Site VPN](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%B5%E3%82%A4%E3%83%88%E9%96%93%E3%81%AEvpn%E6%8E%A5%E7%B6%9A-site-to-site-vpn) 、 [クライアントVPNエンドポイント](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%AF%E3%83%A9%E3%82%A4%E3%82%A2%E3%83%B3%E3%83%88vpn) の作成、一覧表示、設定を行います。

## AWS CLOUD WAN

VPCとオンプレミス間を接続する方法には、前述の [Site-to-Site VPN](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%B5%E3%82%A4%E3%83%88%E9%96%93%E3%81%AEvpn%E6%8E%A5%E7%B6%9A-site-to-site-vpn) 、 [クライアントVPN](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%82%AF%E3%83%A9%E3%82%A4%E3%82%A2%E3%83%B3%E3%83%88vpn) 、 [トランジットゲートウェイ](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%88%E3%83%A9%E3%83%B3%E3%82%B8%E3%83%83%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) 、 [Direct Conntect](https://aws.amazon.com/jp/directconnect/) 等多くのサービスが存在し、これらの個別に管理すると多大な手間が掛かってしまいます。

そこで、これら **複数のオンプレネットワークやVPC間の接続を統合的に管理** できるようにした2021年リリースの新機能が、 **AWS クラウド WAN** です。

詳細は以下の記事が分かりやすいです

※2022/6現在、大阪リージョンでは使用できません

## TRANSIT GATEWAY

[前述の](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#%E3%83%88%E3%83%A9%E3%83%B3%E3%82%B8%E3%83%83%E3%83%88%E3%82%B2%E3%83%BC%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A4) トランジットゲートウェイおよび付随したTransit Gatewayアタッチメント、Transit Gatewayルートテーブルの作成、一覧表示、設定を行います。

## トラフィックのミラーリング

ミラーリングとは、ネットワーク上を流れているデータ (トラフィック)を他の場所にコピーした上で、各種検査を行う事を指します ( [**有料**](https://aws.amazon.com/jp/vpc/pricing/) サービスなのでご注意ください)

検査の目的としては、異常なトラフィックの判別による **攻撃検出** や、監査等に使用するログを残すための **法令対応** 等が挙げられます。

詳細は以下の記事が分かりやすいです

※2022/7現在、大阪リージョンでは使用できません

  
  

## VPCの構築方法

下図のVPC構成 ( [冒頭で紹介した構成](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77#vpc%E3%81%A8%E3%81%AF) )の構築を通じ、VPCの実際の構築方法を解説します。

[![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F42d9caf9-082f-9340-3cb7-490628d5938e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=90bf4155a0af9ca7df55a9399c66f9d9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F610167%2F42d9caf9-082f-9340-3cb7-490628d5938e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=90bf4155a0af9ca7df55a9399c66f9d9)

指針として、以下の **ベストプラクティスに極力従うよう** 構築を進めていきます

長くなったので、 **以下の別記事でまとめました** 。  
**VPC構築を実践する上で役立つ記事** を目指したので、ぜひ **ご一読** いただけると嬉しいです！

[1724](https://qiita.com/c60evaporator/items/2f24d4796202e8b06a77/likers) 1924 [comment 3](https://qiita.com/c60evaporator/items/#comments)