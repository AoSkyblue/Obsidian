---
title: "AWS サービスがどこにあるのかまとめ"
source: "https://qiita.com/saitotak/items/d2ede050e7a2224da46d"
author:
  - "[[Qiita]]"
published: 2020-02-03
created: 2025-04-04
description: "AWS 上のセキュリティや NW 構成を考えるにあたって、どのサービスがどこにあるのか、AZ サービスなのか、リージョンサービスなのか を考える機会が多く、自分の頭の整理のため簡単にまとめます。…"
tags:
  - "clippings"
image: "https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-user-contents.imgix.net%2Fhttps%253A%252F%252Fcdn.qiita.com%252Fassets%252Fpublic%252Farticle-ogp-background-afbab5eb44e0b055cce1258705637a91.png%3Fixlib%3Drb-4.0.0%26w%3D1200%26blend64%3DaHR0cHM6Ly9xaWl0YS11c2VyLXByb2ZpbGUtaW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnFpaXRhLWltYWdlLXN0b3JlLnMzLmFtYXpvbmF3cy5jb20lMkYwJTJGMTMwODc0JTJGcHJvZmlsZS1pbWFnZXMlMkYxNTI1MDgwNTg4P2l4bGliPXJiLTQuMC4wJmFyPTElM0ExJmZpdD1jcm9wJm1hc2s9ZWxsaXBzZSZmbT1wbmczMiZzPTcxOTVjODk4ZjdjNzNjZTU5YTVkZDIzZmNhMTQyZjY5%26blend-x%3D120%26blend-y%3D467%26blend-w%3D82%26blend-h%3D82%26blend-mode%3Dnormal%26s%3Df483ebb4829421fa1cff3fa58ea767ff?ixlib=rb-4.0.0&w=1200&fm=jpg&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTk2MCZoPTMyNCZ0eHQ9QVdTJTIwJUUzJTgyJUI1JUUzJTgzJUJDJUUzJTgzJTkzJUUzJTgyJUI5JUUzJTgxJThDJUUzJTgxJUE5JUUzJTgxJTkzJUUzJTgxJUFCJUUzJTgxJTgyJUUzJTgyJThCJUUzJTgxJUFFJUUzJTgxJThCJUUzJTgxJUJFJUUzJTgxJUE4JUUzJTgyJTgxJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LXBhZD0wJnM9NjE0YWYyODUzNjZhOGFhZWJkNmQ4MmNmMTAyYzVlYjQ&mark-x=120&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTgzOCZoPTU4JnR4dD0lNDBzYWl0b3RhayZ0eHQtY29sb3I9JTIzMUUyMTIxJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1wYWQ9MCZzPWRiM2IyYmIyNzc3MDM2MDM4MjgyZDE4ODM2ZjEwMmQ5&blend-x=242&blend-y=480&blend-w=838&blend-h=46&blend-fit=crop&blend-crop=left%2Cbottom&blend-mode=normal&s=def52689dc4bea4be50b0c6204efd8e2"
PermanentNote:
---
info

この記事は最終更新日から5年以上が経過しています。

[@saitotak (🐑🐑 🐑 🐑 )](https://qiita.com/saitotak) 最終更新日 投稿日 2017年08月28日

　AWS 上のセキュリティや NW 構成を考えるにあたって、どのサービスがどこにあるのか、AZ サービスなのか、リージョンサービスなのか を考える機会が多く、自分の頭の整理のため簡単にまとめます。

## まとめた図

　サービスの「場所」を表現する為の図です、全エッセンスを入れるとゴチャゴチャするので、一部省略しています。  
[![Image](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F130874%2F1a6cf2b2-163b-1e3f-b80c-f8ac5aaf7517.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=10561710f4078ca66deb8bf06b1e85aa)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F130874%2F1a6cf2b2-163b-1e3f-b80c-f8ac5aaf7517.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=10561710f4078ca66deb8bf06b1e85aa)

## 説明

　代表的なもの以外（というか個人的に普段使わないもの）は入れてなかったりします。サービスと呼んでいいのかわからないものも混ぜ込みました。

- AZ サービス
	- Subnet
		- サブネット単位で必要となる NAT Gateway / NACL も AZ サービス
	- EC2
		- AZ単位で設定する EBS / Placement Group も AZ サービス
	- RDS
	- ELB [^1]
	- ElasticCache
	- Redshift
- リージョンサービス
	- VPC
		- VPC毎に必要となる Security Group / VPC Endpoints / VPC Peering / EIP も リージョンサービス
	- Auto Scaling
	- S3 [^2]
		- バックエンドで S3 を利用している EBS Snapshot / AMI も リージョンサービス
	- DynamoDB
	- SQS
	- CloudSearch
	- Lambda [^3]
	- API Gateway <sup><a href="https://qiita.com/saitotak/items/#fn-3">3</a></sup>
	- Code 兄弟
- グローバルサービス
	- IAM
	- Route53
	- CloudFront
	- WAF
	- Key Pair [^4]

　誤りあれば、ご指摘ください。

[620](https://qiita.com/saitotak/items/d2ede050e7a2224da46d/likers) 489 [comment 1](https://qiita.com/saitotak/items/#comments)

[^1]: リージョンサービスにも見えますが、実態は Multi-AZ で AutoScaling なインスタンスです、図示する場合は Subnet の間に書くのがよいです

[^2]: S3 はサービスとしてはグローバルで、データはリージョンに配置されます

[^3]: 指定した VPC / Subnet 内に起動することもできるが、Endpoint（ARN）はリージョン

[^4]: EC2キーペアはリージョン毎、RSA キーペアはグローバル